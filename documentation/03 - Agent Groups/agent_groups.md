# Agent Groups (Agent Teams)

An **agent team** is a named, reusable multi-agent workflow. You define it once; invoke it by name in any session where it is loaded. Teams are the primary unit of reuse in SAILL.

---

## What a Team Is

A team is an ordered list of **roles**. Each role specifies:

- A label (the role's name)
- A model group (the capability tier for the work)
- An optional condition flag (`if needed`, `if asked`, `ask user`, `parallel`)
- A one-line charter describing what the role does

The acting model (the model running the current session) spawns each role in order on its assigned model group, passing the prior role's output as input.

**Invoking a team** is natural-language — no slash command:

> "Send the investigate-and-fix team at the flaky test in auth/."
> "Run a review-and-fix over my current diff, but only fix if I say so."

The acting model matches the name to the loaded team definition and executes the role sequence.

---

## Conditional Flags

A role's model group can include a condition flag, written inline in parentheses:

| Flag | Meaning |
|---|---|
| `(#group, if needed)` | Run only if the acting model judges it adds value; else skip |
| `(#group, if asked)` | Run only when the user explicitly requests it; else skip |
| `(#group, ask user)` | Pause before this role and wait for the user's input or approval |

Conditions combine with loops: a conditional looping role loops only on runs where it actually executes.

> For the full flag set including `parallel`, `wait`, `Loop`, `if fail`, box syntax, skill calls, and `-context-` values, see `agent_team_flags.md`.

---

## Loop Constructs

A role can declare a loop to retry an earlier part of the workflow on failure. The looping role sends structured feedback back to a named earlier role and re-runs from there.

Four required parts of a loop declaration:

1. **Loop-back target** — which earlier role to re-run (use the role's label, not its step number — renumbering won't break it)
2. **Loop condition** — what counts as failure and triggers another pass
3. **Max iterations** — a hard cap; always set one
4. **Cap action** — what happens if the cap is reached without passing: `report-failure` (stop and surface failures) or `proceed` (continue with caveat)

**Loop syntax** (inline on the looping role):

```
**Loop:** on <condition>, return feedback to "<role>" and re-run from there;
repeat until <pass condition> or <max> iterations, then <cap action>.
```

**Example** (from the Full Team's Validator):

```
5. Validator (#midcost, if asked) — verifies the Implementer's work and checks for regressions.
   **Loop:** on failure, return specific feedback to the Implementer and re-run from "Implementer";
   repeat until the Validator passes clean or 3 iterations, then stop and report any
   outstanding failures.
```

---

## The Five Shipped Starter Teams

### Investigate & Fix

Diagnose a problem then apply the fix.

| # | Role | Model group | Charter |
|---|---|---|---|
| 1 | Investigate | #midcost | Diagnoses root cause across relevant files/logs; hands a precise diagnosis to Fix |
| 2 | Fix | #lowcost | Applies the change described by Investigate and verifies the issue is resolved |

### Full Team

Full lifecycle for a sizable or ambiguous task. Default when "send an agent team" is used without a specific name.

| # | Role | Model group | Charter |
|---|---|---|---|
| 1 | Orchestrator | #highcap | Breaks the task down and coordinates the rest |
| 2 | Log-reader | #lowcost, if needed | Gathers runtime evidence before planning |
| 3 | Planner | #highcap, ask user | Designs the approach; presents plan and waits for user approval |
| 4 | Implementer | #lowcost | Writes the code |
| 5 | Validator | #midcost, if asked | Verifies Implementer's work; loops back to Implementer on failure (cap: 3) |

### Review & Fix

Audit a diff then apply findings.

| # | Role | Model group | Charter |
|---|---|---|---|
| 1 | Reviewer | #highcap | Audits the diff for correctness, security, and regressions; hands findings to Fixer |
| 2 | Fixer | #lowcost | Applies the reviewer's findings and confirms each is resolved |

The Fixer role includes a loop:
> **Loop:** on remaining issues, return feedback to "Reviewer" and re-run from there; repeat until clean or 2 iterations, then report unresolved items.

### Explore & Summarize

Fan out across a codebase or question, then distill.

| # | Role | Model group | Charter |
|---|---|---|---|
| 1 | Orchestrator | #highcap | Defines scope and distributes work |
| 2 | Recon box | #investigate, parallel | CodeReader and DocReader run concurrently inside a `Recon[ … ]` box — `Recon[ CodeReader (#investigate, parallel), DocReader (#investigate, parallel) ] (wait)` |
| 3 | Synthesizer | #midcost | Integrates all findings into a single actionable report |

The `Recon[ … ]` box is the reference example of the box syntax — two parallel roles grouped into one node, with a `(wait)` sync point after.

### Build & Certify

Build an artifact then run full certification, with a bounded retry loop and failure escalation.

| # | Role | Model group | Charter |
|---|---|---|---|
| 1 | Builder | #lowcost | Builds the artifact |
| 2 | Certifier | #midcost | Runs the certification suite |

The Certifier includes a loop with failure escalation:
> **Loop:** on failure, return findings to "Builder" and re-run from there; repeat until certified or `-context:cap-` iterations, then report failures.
> `if fail /escalate_cert_failure`

This is the reference example of `-context:cap-` (a runtime-resolved iteration cap) and `if fail` (failure escalation to a named skill).

---

## Directing Roles at Invocation Time

A team is a scaffold, not a cage. At invocation you can add role direction in plain language on top of the team definition:

> "Send a full-team for the export bug; have the Validator run /security-review, and Log-reader scope to just the auth module."

SAILL flags govern control flow (whether/when/how often a role runs). A role's specific work — which tools to use, what to focus on — is charter prose, set or overridden in plain language at invocation. Do not encode role-work direction as SAILL primitives.

---

## Defining Custom Teams

Custom teams go in `local.agent_teams.md` (gitignored — keeps local customizations out of version control). Same-name entries override the shipped file; new names are unioned in. Never edit the shipped `agent_teams.md` directly.

Minimal format:

```markdown
### my-team-name
Short description of what this team does.

1. Role A (#midcost) — one-line charter
2. Role B (#lowcost) — one-line charter
```

With a condition and loop:

```markdown
### build-and-verify
1. Builder (#lowcost) — builds the artifact.
2. Verifier (#midcost, if asked) — runs verification suite.
   **Loop:** on failure, return findings to "Builder" and re-run from there;
   repeat until clean or 3 iterations, then report failures.
```

Use the `/agent-teams` skill to add or modify teams without hand-editing. It handles the file seam, scope selection, and format consistency.

---

## Scope Cascade for Team Definitions

Team definitions cascade from least to most specific. Most-specific wins on same-name.

```
<root>/agent_teams.md                       (shipped base)
  <root>/agent_teams.local.md               (root overrides)
    <project>/agent_teams.local.md
      <workspace>/agent_teams.local.md
        <subfolder>/agent_teams.local.md    (most specific)
```

To activate overrides at a scope: the directory's `agents.md` must `@-import` its sibling `local.agent_teams.md`. This is handled automatically by the `/agent-teams` skill when creating a new scope.

---

## The /agent-teams Skill

Tooling to define and override agent teams without hand-editing markdown blind.

**Usage:**
- `/agent-teams` (bare) — resolve and display teams currently in effect; ask whether to add or modify
- Add scope argument to target a specific level (project, brain, subfolder, cwd)

**What it does:**
1. Runs `resolve_agent_teams.py` to show currently active teams and their sources
2. Identifies the correct `local.agent_teams.md` file for the chosen scope
3. Ensures the scope's `agents.md` `@-imports` the local file
4. Writes the new or modified team definition in the correct format

**Never edits the shipped `agent_teams.md`.**

See also: `/test-agent-teams` to verify that spawned roles actually run on their intended model groups.
