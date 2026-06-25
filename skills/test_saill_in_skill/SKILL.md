---
name: test_saill_in_skill
description: Proof-of-concept SAILL-in-skill. Replaces a wordy dirtree-check skill with a tight SAILL block. Audits tested_implementations nonce coverage via the Nonce Trace Audit team.
tools: Read, Bash, Agent
---

# Skill: /test_saill_in_skill

**Model preference:** `#midcost`

"For every dir with a trace nonce file, send a haiku agent to report agents.md files loaded."

### Nonce Trace Audit

1. Scanner (#lowcost) — globs `**/testing_nonce.md` under -context:root-; emits directory list as -context:dirs-.
2. Readers[ Nonce-Reader (#lowcost, parallel) per -context:dirs- ] (wait) — one haiku agent per dir; reads testing_nonce.md and walks parent dirs for agents.md trace lines; returns CSV row: `dir,trace1|trace2|...,nonce`.
3. Synthesizer (#midcost) — collects CSV rows; builds fenced markdown directory tree (nonce ✓, chain depth per node, MISSING flagged).
4. Writer (#lowcost) — writes tree to `documentation/lastrun_dirtree_check.md` with run header; appends `sha256: <hash>`.

**Send the Nonce Trace Audit team against -context-**
