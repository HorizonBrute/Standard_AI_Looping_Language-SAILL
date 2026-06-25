# Agent Teams — Horizon AIOS

---

When asked to spawn a named team, look it up below (local overrides win), then spawn each role in order on its model group, chaining output. If no name matches, use **Full Team**.

## References
Standard AI Looping Language (SAILL) in use:
SAILL Flags Reference: `./agent_team_flags.md`

## Teams

### Investigate & Fix
Diagnose a problem then apply the fix.

1. Investigate (#midcost) — diagnoses root cause across the relevant files/logs; hands
   a precise diagnosis and proposed change to the Fix role.
2. Fix (#lowcost) — applies the change described by Investigate and verifies it resolves
   the issue.

### Full Team
Full lifecycle for a sizable or ambiguous task. This is the default that the generic
phrase "send an agent team" resolves to.

1. Orchestrator (#highcap) — breaks the task down and coordinates the rest.
2. Log-reader (#lowcost, if needed) — gathers runtime evidence before planning.
3. Planner (#highcap, ask user) — designs the approach; presents plan and waits for approval.
4. Implementer (#lowcost) — writes the code.
5. Validator (#midcost, if asked) — verifies the Implementer's work and checks for regressions.
   **Loop:** on failure, return specific feedback to the Implementer and re-run from "Implementer";
   repeat until the Validator passes clean or 3 iterations, then stop and report any
   outstanding failures.

### Review & Fix
Audit a diff then apply findings.

1. Reviewer (#highcap) — audits the diff for correctness, security, and regressions;
   hands a findings list to the Fixer.
2. Fixer (#lowcost) — applies the reviewer's findings and confirms each is resolved.
   **Loop:** on remaining issues, return feedback to "Reviewer" and re-run from there;
   repeat until clean or 2 iterations, then report unresolved items.

### Explore & Summarize
Fan out across a codebase or question, then distill.

1. Orchestrator (#highcap) — defines scope and distributes work.
2. Recon[ CodeReader (#investigate, parallel), DocReader (#investigate, parallel) ] (wait)
3. Synthesizer (#midcost) — integrates all findings into a single actionable report.

### Build & Certify
Build an artifact then run full certification, with a bounded retry loop and failure escalation.

1. Builder (#lowcost) — builds the artifact.
2. Certifier (#midcost) — runs the certification suite.
   **Loop:** on failure, return findings to "Builder" and re-run from there;
   repeat until certified or -context:cap- iterations, then report failures.
   if fail /escalate_cert_failure

---

## Custom teams

Use `local.agent_teams.md` (gitignored). Same-name overrides this file; new names are unioned in.

---

## Scope Precedence

Same cascade and override-file rules as `horizon_aios_model_prefs.md § Scope Precedence`. No new semantics.
