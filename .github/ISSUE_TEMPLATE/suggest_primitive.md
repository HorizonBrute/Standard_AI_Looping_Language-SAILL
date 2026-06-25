---
name: Suggest New Primitive
about: Propose a new SAILL primitive or flag for the core language
title: '[PRIMITIVE] '
labels: primitive-proposal
assignees: ''
---

> Before submitting: read [Proposing a New Primitive](../../documentation/13%20-%20Proposing%20a%20New%20Primitive/proposing_primitive.md) for the full how-to and the bar that must be cleared. Proposals without test evidence will not be merged.

---

## Origin Prompt

<!-- The natural-language prompt that surfaced the need for this primitive.
     This is the human expression of what the primitive encodes. -->

> 

## Why No Existing Primitive Covers This

<!-- One or two sentences. Show you've checked: if needed, if asked, ask user,
     parallel, wait, Loop, if fail, [ ] box, -context:<value>-, /skill-name -->

## Proposed Primitive

**Flag name:** `your-flag`

**Form:** `inline` or `annotation`

**Definition (one sentence):** 

```markdown
| `your-flag` | inline | One sentence: what the acting model does when it sees this flag. |
```

## Team Definition Using the Primitive

<!-- A complete agent team that exercises the new primitive in a real, useful workflow. -->

```
### Team Name

1. Role-One (#midcost) — charter.
2. Role-Two (#lowcost, your-flag) — charter.
3. Role-Three (#midcost) — charter.
```

## Test Evidence

<!-- Output from /test-agent-teams or equivalent.
     Must show: each role executed, the flagged role behaved as described,
     and the model assignments. If the flag controls conditional execution,
     show both the triggered and skipped cases. -->

```
<!-- paste output here -->
```

## Before / After Compression

**Before — natural language in team definition:**
```
<!-- the verbose form that required re-explaining the behavior each time -->
```

**After — SAILL with the new primitive:**
```
<!-- the compressed form using your proposed flag -->
```

**Estimated token reduction:** 

## Invoke Example

<!-- One line showing how a user would invoke a team using this primitive -->

> "Send the [Team Name] team against [target]."
