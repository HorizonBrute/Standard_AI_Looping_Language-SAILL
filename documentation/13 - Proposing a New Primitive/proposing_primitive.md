# Proposing a New Primitive

SAILL's primitive set is deliberately small. The primitives are in the always-loaded chain — every byte costs every session, for every user. A new primitive must clear a high bar before it belongs in the shared vocabulary.

This document is the how-to for building and submitting that case.

---

## The Bar

A proposed primitive must satisfy all three:

| Criterion | What it means |
|---|---|
| **Inexpressible** | Cannot be covered cleanly by composing existing primitives. If it can, the answer is "write a team definition, not a new flag." |
| **General** | Useful across many team types and domains — not a workaround for one specific workflow. |
| **Terse** | Expressible in one or two words. If naming it takes a sentence, it isn't a primitive. |

The existing primitives for reference: `if needed`, `if asked`, `ask user`, `parallel`, `wait`, `Loop`, `if fail`, `[ … ]` box, `-context:<value>-`, `/skill-name`. Before proposing, confirm none of these — alone or composed — already covers your case.

---

## Step 1 — Define the Prototype Locally

Add a new row to `agent_team_flags.local.md` in your working directory. This file is gitignored, local to your setup, and loaded alongside the base `agent_team_flags.md`.

Use the same table format as the base file:

```markdown
# Agent Team Flags — Local Extensions

| Flag | Form | Means |
|------|------|-------|
| `your-flag` | inline | One sentence: what the acting model does when it sees this flag. |
```

Keep the definition to one sentence. If you need more than a sentence, the primitive is not terse enough yet.

---

## Step 2 — Write a Team That Uses It

Write an agent team definition that exercises the prototype primitive in a real, useful workflow. Add it to `local.agent_teams.md`.

```markdown
### My Prototype Team

1. Role-One (#midcost) — does the work.
2. Role-Two (#lowcost, your-flag) — behavior controlled by the new primitive.
3. Role-Three (#midcost) — synthesizes.
```

The team must:
- Use the new primitive on at least one role
- Represent a workflow that would actually be useful to others
- Not be a contrived test constructed solely to make the flag appear

---

## Step 3 — Run the Test

Invoke `/test-agent-teams` and select your prototype team. The skill spawns each role as a real sub-agent and verifies execution against a nonce.

Capture the output. At minimum, record:
- That each role executed (nonce confirmed)
- That the role carrying the new primitive behaved as your flag definition described
- Which model each role ran on

If the primitive controls whether a role runs (like `if needed`), run the team twice: once where the condition should trigger and once where it should not. Verify both.

---

## Step 4 — Document the Origin Prompt

Record the natural-language prompt that first surfaced the need for this primitive. This is the human-language expression of what the primitive encodes.

Example:

> **Origin prompt:** "If the build output is cached and hasn't changed since last run, skip the expensive validation step."

This prompt is part of the submission — it shows the real-world need and provides the plain-English definition.

---

## Step 5 — Show the Compression

Show the before/after: the full natural-language prompt that required this behavior, versus the SAILL expression using the new primitive.

**Before (natural language in team definition):**
```
2. Validator (#midcost) — runs only if the build output hash differs from the last
   recorded hash in .build_cache; if hashes match, skip this role entirely and
   proceed to the next step.
```

**After (SAILL with the new primitive):**
```
2. Validator (#midcost, if-changed) — verifies build output.
```

Measure the token difference. A meaningful primitive compresses meaningfully.

---

## Step 6 — Submit

Open an issue using the **Suggest New Primitive** template. The submission package:

| Item | What to include |
|---|---|
| **Origin prompt** | The natural-language prompt that created the need |
| **Primitive definition** | Flag name, form (inline/annotation), one-sentence meaning |
| **Team definition** | The team that uses it, as a markdown block |
| **Test evidence** | Output from `/test-agent-teams` showing the primitive ran and behaved correctly |
| **Before/after** | Full prompt vs. SAILL expression; estimated token reduction |
| **Existing primitive check** | One-line statement of why no existing primitive covers this |

A submission without test evidence will not be merged. The bar is not "this flag seems useful" — it is "this flag was tested, behaves predictably, and cannot be expressed any other way."

---

## See Also

- [SAILL Language Guide](../07%20-%20SAILL%20Language%20Guide/saill_guide.md) — the full current primitive set
- [Evaluating Context Cost](../05%20-%20Evaluating%20Context%20Cost/context_cost.md) — the Terseness Contract and token economy
- [SAILL Inside Skills](../12%20-%20SAILL%20Inside%20Skills/saill_in_skills.md) — the before/after pattern for measuring compression
- [CONTRIBUTING.md](../../CONTRIBUTING.md) — contribution policy
