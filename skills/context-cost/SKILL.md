---
name: context-cost
description: Report harness context overhead (KB, words, estimated tokens) for a given path by walking the directory tree and collecting all CLAUDE.md, agents.md, and @-import files the harness auto-loads. Use when the user types /context-cost or /ctx-cost, or wants to know what files are loading into context.
tools: Bash
---

# Skill: /context-cost

**Model preference:** `#lowcost`

Report how much context overhead the harness will auto-load for a given path — walking up the directory tree and collecting every `CLAUDE.md`, `CLAUDE.local.md`, `agents.md`, and `@`-import file pulled in at session start.

---

## Arguments

`/context-cost [path]`

- `path` — optional; defaults to the current working directory if omitted.

---

## Step-by-step execution

### Step 1 — Locate the script

Find `context_cost.py` in the `bin/` directory of the SAILL repo. If you are running from within a tested_implementation directory, the script is at `../../bin/context_cost.py` relative to your working directory. If running from the repo root, it is at `bin/context_cost.py`.

Run:
```
python <path-to-bin>/context_cost.py <target-path> --json
```

If the script is not found, report its expected location and stop.

### Step 2 — Parse and format the output

The JSON structure is:
```json
{
  "path": "...",
  "files": [
    {"level": N, "path": "...", "kb": N, "words": N, "tokens": N, "imported_by": "..." or null}
  ],
  "total_kb": N,
  "total_words": N,
  "total_tokens": N
}
```

### Step 3 — Present the report

3.1 Print: `Context overhead for: <path>`

3.2 Print a table sorted by `level` (outermost first):
```
Level  File                              KB     Words   ~Tokens
-----  --------------------------------  -----  ------  -------
  0    CLAUDE.md                         1.2    210     280
  1    agents.md                         3.4    580     775
       (imported by CLAUDE.md)
```

Show `(imported by <basename>)` on a sub-line when `imported_by` is non-null.

3.3 Print totals:
```
Total: N files — X.X KB — Y words — ~Z tokens
```

3.4 Threshold flags:
- If `total_tokens` >= 20000: `[WARN] High context load: ~Z tokens. Consider trimming @-imports.`
- Else if `total_tokens` >= 8000: `[NOTE] Moderate context load: ~Z tokens. Worth reviewing.`
- Otherwise: no flag.
