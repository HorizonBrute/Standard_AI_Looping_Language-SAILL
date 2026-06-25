---
name: try-my-prompt
description: Take a natural-language prompt and return two outputs — an agent_teams.md SAILL entry and a terse one-line invocation — by invoking /convert-prompt-to-saill and formatting the result.
tools: Read, Bash
---

# Skill: /try-my-prompt

**Model preference:** `#midcost`

Given a natural-language prompt, invoke `/convert-prompt-to-saill` to do the conversion, then present the result in two clearly-labelled sections.

---

## Steps

1. Receive the user's prompt. If none was supplied, ask for it.

2. Invoke `/convert-prompt-to-saill <prompt>`. Do NOT do the conversion yourself — hand off entirely to that skill.

3. From the output of step 2, present two sections:

---

### Section 1 — agent_teams.md entry

The `### TeamName` + numbered roles block, exactly as it would appear in `agent_teams.md`. Render it in a fenced code block. This is the copy-paste-ready SAILL form.

---

### Section 2 — One-Line Invocation

A single terse line showing how to invoke the team once it is defined. This is the post-implementation invocation — the payoff of naming the team.

Example shape:
```
"Send the Investigate & Fix team at -context-scope-."
```

Keep it to one line. Surface any `-context-` slots the caller must fill in.

---

## Notes

- This skill only orchestrates and formats. All SAILL logic lives in `/convert-prompt-to-saill`.
- Section 1 is the definition; Section 2 is the invocation. Terseness is the point.
- After presenting both sections, offer the same next steps that `/convert-prompt-to-saill` would: save (`/agent-teams`), run, or test (`/test-agent-teams`).
