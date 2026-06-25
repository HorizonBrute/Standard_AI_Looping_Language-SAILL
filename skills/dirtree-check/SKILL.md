---
name: dirtree-check
description: Walk every tested_implementation directory that has a testing_nonce.md, collect each directory's agents.md inheritance chain and nonce via parallel haiku agents, render a directory tree showing context depth at every node, write it to documentation/lastrun_dirtree_check.md, and append the file's SHA256 hash. Use when asked to run the directory tree check, verify context inheritance, or audit nonce coverage.
tools: Read, Bash, Agent
---

# Skill: /dirtree-check

**Model preference:** `#midcost`

Verify that every instrumented directory in `tested_implementations/` has a nonce and correct agents.md inheritance chain. Output a rendered tree to the documentation root and stamp it with a SHA256 hash.

---

## Step 1 — Discover directories

```bash
find working_copy/tested_implementations -name "testing_nonce.md" -type f
```

Or use Glob: `**/testing_nonce.md` under `working_copy/tested_implementations/`.

Collect the parent directory of each hit. That is the full target list.

---

## Step 2 — Collect nonce + agents.md chain per directory (parallel)

For each directory, spawn a **haiku** sub-agent with this brief:

> Read `<dir>/testing_nonce.md` — extract the value after `nonce: `.
> Read each `agents.md` file in the inheritance chain (the directory itself, then each
> parent directory up to the impl root that has an `agents.md`). From each, extract the
> first line after `# This context loaded from: `.
> Return ONE CSV row, no header, no other output:
> `directory_shortname,trace1|trace2|...,nonce`
> Traces ordered deepest-first.

Pre-compute the inheritance chain for each directory and pass it explicitly — do not ask
the haiku agent to discover it. Walk the path from the target directory up to the impl root,
collecting any directory that has an `agents.md`.

Spawn all sub-agents in a **single parallel batch** (one Agent call per directory, all in
one message). Collect all CSV rows before proceeding.

---

## Step 3 — Build the directory tree markdown

From the collected rows, render a markdown fenced code block using tree characters
(`├──`, `└──`, `│`). For each node show:

- directory name, right-aligned nonce label `nonce: <value> ✓`
- `agents.md chain (<N> level(s)):` with each trace on its own indented line

Group by implementation: impl 1, impl 2 (full hierarchy), impl 3.

Append a summary table:

| Impl | Directories checked | Max inheritance depth | All nonces confirmed |

---

## Step 4 — Write output file

Write the complete tree to:
```
working_copy/documentation/lastrun_dirtree_check.md
```

Include at the top:
```
# Directory Tree Context Check
Run: <ISO date> | <N> haiku agents | working_copy/tested_implementations
```

---

## Step 5 — SHA256 hash

After writing the file, compute its SHA256:

```bash
python -c "import hashlib; d=open('working_copy/documentation/lastrun_dirtree_check.md','rb').read(); print(hashlib.sha256(d).hexdigest())"
```

Append to the bottom of the file:

```
---
sha256: <hash>
```

Then recompute and verify the hash matches before reporting done. If the file was modified
by the append, the final hash is the hash of the file **with** the sha256 line included —
write a placeholder, compute, replace, verify once.

---

## Step 6 — Report

Print a one-line confirmation:
```
dirtree-check complete — <N>/15 nodes ✓  sha256: <first-8-chars>...
```

Do not print the full tree to the conversation — it is in the output file.

---

## Notes

1. The inheritance chain for impl 2 stacks up to 4 levels deep at project leaves. Impl 3
   directories are siblings with no parent chain. Impl 1 is a single-level flat directory.
2. If any nonce file is missing or any agents.md trace line is absent, flag the directory
   as `MISSING` in the tree and note it in the report.
3. The SHA256 is of the final file bytes including the `sha256:` line itself. This is a
   self-referential hash — compute it by writing the placeholder, computing, and replacing
   in one edit so the file is never re-read after hash insertion.
   Practical approach: write the file without the hash line, compute hash, append the line.
   The hash then covers the full file content.
