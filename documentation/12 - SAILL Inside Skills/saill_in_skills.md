# SAILL Inside Skills

SAILL notation can be embedded directly in a skill body (`SKILL.md`) to define and direct multi-agent execution. This replaces verbose prose role instructions with a compact SAILL team block — same behavior, far fewer tokens.

**Status: Tested.** See reference skill: `skills/test_saill_in_skill/SKILL.md`

---

## The Pattern

A skill that spawns agents traditionally describes each role in prose — what to do, in what order, with what output. That description is verbose, hard to scan, and re-explains SAILL concepts the acting model already knows.

The **SAILL-in-skill** pattern instead:

1. Names a team inline in the skill body using standard SAILL notation
2. Closes with a single dispatch line: `Send the <Team Name> team against -context-`
3. Lets the acting model execute the SAILL block directly, with no additional prose

The model treats embedded SAILL exactly as it treats a team defined in `agent_teams.md` — the primitives, boxes, flags, and context placeholders carry the same meaning.

---

## Before and After

Live comparison:

| Version | File |
|---|---|
| Before — verbose prose (~50 lines) | [`skills/test_saill_in_skill/example_conversion.md`](../../skills/test_saill_in_skill/example_conversion.md) |
| After — SAILL-in-skill (~10 lines) | [`skills/test_saill_in_skill/SKILL.md`](../../skills/test_saill_in_skill/SKILL.md) |

Same four roles. Same execution order. Same output contract. Roughly 80% fewer tokens.

---

## How to Author a SAILL-in-Skill

### Structure

```markdown
---
name: my-skill
description: ...
tools: ...
---

# Skill: /my-skill

**Model preference:** `#<group>`

<one-line natural-language intent>

### <Team Name>

1. Role-One (#<group>) — <charter>.
2. Role-Two (#<group>, parallel) — <charter>.
3. Role-Three (#<group>) (wait) — <charter>.

**Send the <Team Name> team against -context-**
```

### Rules

| Rule | Detail |
|---|---|
| Team name | A unique, readable label — used in the dispatch line and in logs |
| Role charters | Keep to one sentence; the SAILL structure carries the order/concurrency — don't re-explain it in prose |
| `-context-` placeholders | Use qualified forms (`-context:root-`, `-context:dirs-`) for clarity |
| Dispatch line | Always the last line of the skill body: `**Send the <Team Name> team against -context-**` |
| Model preference | Declare at the top of the skill; the team's per-role groups can override for individual roles |

### What you can use

All standard SAILL primitives work inside a skill block:

- `parallel` / `wait` — concurrency and sync
- `Loop` — retry with feedback
- `if needed` / `if asked` / `ask user` — conditional execution and gates
- `[ … ]` boxes — sub-team grouping and nesting
- `-context:<qualifier>-` — runtime-resolved values
- `if fail <action>` — failure handlers

---

## Why This Works

The acting model (the skill dispatcher) reads the skill body as its operating instructions. SAILL primitives are part of its loaded context vocabulary — defined in `agent_teams_flags.md` and documented in the SAILL Language Guide. There is no additional wiring required: the skill body *is* the team definition.

The key insight: a skill does not need to be in `agent_teams.md` to use SAILL. Any text the model receives as instructions can carry SAILL notation, and the model will execute it.

---

## Token Economy

| Version | Approximate lines | Cognitive load |
|---|---|---|
| Prose role descriptions | ~50 | High — redundant with SAILL primitives the model already knows |
| SAILL-in-skill | ~10 | Low — terse, scannable, self-documenting |

The reduction comes from eliminating explanations of *what the primitives mean* — the model already knows. A SAILL block says the same thing in the language the model natively operates in.

This is the "define-once, use-many" principle applied to skills: the SAILL vocabulary is defined once in `agent_teams_flags.md`; skills reference it without re-explaining it.

---

## Reference Implementation

The tested proof-of-concept is at `skills/test_saill_in_skill/`:

- [`SKILL.md`](../../skills/test_saill_in_skill/SKILL.md) — the SAILL-compressed skill (~10 lines)
- [`example_conversion.md`](../../skills/test_saill_in_skill/example_conversion.md) — the original verbose version (~50 lines)

`SKILL.md` runs the **Nonce Trace Audit** team: Scanner → parallel Readers → Synthesizer → Writer. It produces the same output as the prose version it replaced.

To run it: invoke `/test_saill_in_skill` in a session that has loaded the SAILL working copy context.

---

## See Also

- [SAILL Language Guide](../07%20-%20SAILL%20Language%20Guide/saill_guide.md) — full primitive reference
- [Agent Groups](../03%20-%20Agent%20Groups/agent_groups.md) — shipped teams and how teams are defined
- [Evaluating Context Cost](../05%20-%20Evaluating%20Context%20Cost/context_cost.md) — token economy and the Terseness Contract
- [convert-prompt-to-saill](../../skills/convert-prompt-to-saill/SKILL.md) — skill for converting prose to SAILL
