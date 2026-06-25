---
name: try-my-prompt
description: Take a natural-language prompt and return two ready-to-use outputs — an agent_teams.md entry and a friendly plain-English version — by invoking /convert-prompt-to-saill and formatting the result.
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

### Section 2 — Friendly Prompt

A human-readable, natural-language version of the same flow. Use `***bold-italic***` headings for each role or logical section. Write it as a plain English prompt a user could paste directly to invoke the team.

Example shape:
```
***Investigate & Fix***

***Investigator:*** Analyze the codebase to diagnose the root cause of -context-issue-.

***Fixer:*** Apply the changes described by the Investigator to resolve the issue.
```

---

## Notes

- This skill only orchestrates and formats. All SAILL logic lives in `/convert-prompt-to-saill`.
- Keep both sections terse. The value is the contrast: machine-readable SAILL vs. human-readable gloss.
- After presenting both sections, offer the same next steps that `/convert-prompt-to-saill` would: save (`/agent-teams`), run, or test (`/test-agent-teams`).
