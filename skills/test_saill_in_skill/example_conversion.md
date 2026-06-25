---

### Role 1 — Scanner (in Haiku)

Glob for `**/testing_nonce.md` under in "working copy". Return the list of parent directories
as parent directories. Pass that list to the Readers box.

---

### Role 2 — Readers box (wait)
Spawn **one haiku Nonce-Reader per directory** in `-context:dirs-`, all in a single parallel
batch. Each Nonce-Reader receives:

- Its assigned directory path
- The pre-computed inheritance chain: every parent directory up to the impl root that
  contains an `agents.md` (walk the path upward; stop at the impl root boundary)

Each Nonce-Reader returns ONE CSV row:
```
directory_shortname,trace1|trace2|...,nonce
```
- `directory_shortname` — relative path from `tested_implementations/`
- `trace1|trace2|...` — "This context loaded from:" values, deepest first
- `nonce` — value after `nonce: ` in the nonce file

Wait for all Nonce-Readers before continuing.

---

### Role 3 — Synthesizer in Sonnet

Collect all CSV rows. Build a markdown fenced directory tree using `├──` / `└──
For each node show: directory name, `nonce: <value> ✓`, and the agents.md chain depth.
Group by implementation (impl 1, impl 2 full hierarchy, impl 3). Append summary
Flag any `MISSING` nonce or trace line explicitly.

---

### Role 4 — Writer  in Haiku

Write to: `working_copy/documentation/lastrun_dirtree_check.md`

Header:
```
# Nonce Trace Audit — before_prod run
Run: <ISO date> | Nonce Trace Audit team | working_copy/tested_implementations
```

After writing, compute SHA256 hash of file and append as footer

Report one line to the user:
```
Nonce Trace Audit complete — <N> nodes ✓  sha256: <first-8>...
```
