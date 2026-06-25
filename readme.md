# SAILL — Standard AI Looping Language

A compact, vendor-neutral notation for defining reusable multi-agent workflows.
Define a workflow once, name it, invoke it by name — or share it with anyone using a compatible agent harness.
Think of it as SQL for agent-loop definitions.

---

## The Problem

Running multiple AI agents in a coordinated workflow today means writing a long natural-language prompt every time, or building bespoke tooling that hard-codes the flow. Both approaches are non-portable, expensive in tokens, and not shareable. Changing the workflow means rewriting the prompt or the code.

SAILL solves this by letting you express the workflow once as a named team definition, then invoke it in one line — and share the exact same definition with anyone else on a compatible harness.

---

## Quick Example

**Human prompt:** "Apply the linting fixes and keep retrying until the suite is clean — give up after 3 attempts."

```
### Audit & Fix

1. Implementer (#lowcost) — applies the change.
2. Validator (#midcost) — runs the audit suite against the change.
   **Loop:** on failure, return specific findings to "Implementer" and re-run from there;
   repeat until the Validator passes clean or 3 iterations, then stop and report
   any outstanding failures.
```

**Invoke:** `"Send the Audit & Fix team at the current diff."`

That definition is portable: drop it into any project's `agent_teams.md`, adjust the role charters, keep the control-flow structure. The model does all interpretation — no SAILL-specific tooling is required in the harness.

---

## Key Concepts

| Concept | Description |
|---|---|
| **Agent team** | A named, ordered list of roles with model groups and control-flow flags |
| **Role** | A single unit of work: label + model group + one-line charter |
| **Model group** | A named capability tier (`#lowcost`, `#midcost`, `#highcap`, `#investigate`) mapped to your actual model IDs — defined once, referenced everywhere |
| **SAILL primitive** | A control-flow keyword: `if needed`, `if asked`, `parallel`, `wait`, `Loop`, `ask user` |
| **Scope cascade** | Team and model definitions stack from a root directory down to the current folder; most-specific wins |

---

## Getting Started

1. Clone this repo.
2. Open your agent harness (Claude Code, OpenAI Codex CLI, or any harness that reads a context manifest) with `tested_implementations/1 - single-folder_basic_implementation/` as the working directory.
3. Add `@-import` lines in your `agents.md` pointing at `agent_teams.md`, `agent_team_flags.md`, and `model_prefs.md`.
4. Create `model_prefs.local.md` and fill in your model IDs for each group.
5. Run `/test-agent-teams` to verify real sub-agents spawn and chain correctly.

Full walkthrough: [Overview and Getting Started](documentation/01%20-%20Overview%20and%20Getting%20Started/overview.md)

---

## Documentation

| # | Document | Description |
|---|---|---|
| 00 | [Major Features](documentation/00%20-%20Major%20Features/major_features.md) | Defining capabilities — SAILL-in-skill compression, define-once-use-many, vendor-neutral model groups, native parallelism, scope cascade |
| 01 | [Overview and Getting Started](documentation/01%20-%20Overview%20and%20Getting%20Started/overview.md) | What SAILL is, why it exists, key concepts, prerequisites, and verification steps |
| 02 | [Tested Implementation 1](documentation/02%20-%20Tested%20Implementation%201/impl1.md) | Single-folder basic setup: files, wiring, known @-import limitation, and how to verify |
| 03 | [Agent Groups](documentation/03%20-%20Agent%20Groups/agent_groups.md) | Defining and invoking teams, shipped examples, conditions, loops, custom teams, scope cascade |
| 04 | [Example Loops](documentation/04%20-%20Example%20Loops/example_loops.md) | Twelve annotated copy-paste team definitions covering every SAILL primitive |
| 05 | [Evaluating Context Cost](documentation/05%20-%20Evaluating%20Context%20Cost/context_cost.md) | What loads, the unconditional loading rule, token economy rules, and the Terseness Contract |
| 06 | [How it Works](documentation/06%20-%20How%20it%20Works/how_it_works.md) | Context loading mechanics, @-import rules, scope stacking, and execution flow |
| 07 | [SAILL Language Guide](documentation/07%20-%20SAILL%20Language%20Guide/saill_guide.md) | Full primitive set, naming rules, boxes/nesting, -context- values, failure handlers |
| 08 | [Model Preferences](documentation/08%20-%20Model%20Preferences/model_preferences.md) | Model groups, member grammar, per-session slots, task-class routing, scope cascade |
| 09 | [Tested Implementation 2](documentation/09%20-%20Tested%20Implementation%202/impl2.md) | Multi-folder context inheritance: hierarchy, what stacks at each level, override mechanics |
| 10 | [Tested Implementation 3](documentation/10%20-%20Tested%20Implementation%203/impl3.md) | Environment variable paths for flexible or shared deployments |
| 12 | [SAILL Inside Skills](documentation/12%20-%20SAILL%20Inside%20Skills/saill_in_skills.md) | Embedding SAILL team notation directly in a skill body — authoring rules and token economy |

Full index: [documentation/index.md](documentation/index.md)

---

## Contributing

SAILL is a new open standard — community adoption and feedback are what make it useful.

**What's most wanted right now:**

1. **Good example loops** — real workflows you use or have used, expressed as SAILL team definitions. Submit them as a PR adding a file to `documentation/04 - Example Loops/` or extending `example_loops.md`. Include the human-prompt origin and an invoke line.

2. **Proposals for new primitives or flags** — if you hit a control-flow pattern that doesn't fit the current primitive set, open an issue or PR describing the use case. The bar is: does it fit the "primitives-first, low context overhead" philosophy? Terse wins; richness is expressed by composition.

First-time contributors are welcome. If you're unsure whether your example or proposal fits, open a discussion — the goal is a useful standard, not a closed one.
