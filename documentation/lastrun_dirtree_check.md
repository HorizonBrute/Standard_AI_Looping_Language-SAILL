# Nonce Trace Audit — SAILL-in-skill run
Run: 2026-06-25 | Nonce Trace Audit team | working_copy/tested_implementations

```
tested_implementations/
│
├── 1 - single-folder_basic_implementation/         nonce: impl1-basic-root ✓
│   └── agents.md chain (1):
│       └── 1 - single-folder_basic_implementation\agents.md
│
├── 2 - multi-folder_inheritance_implementation/
│   └── Org_Root/                                   nonce: impl2-org-root ✓
│       ├── agents.md chain (1):
│       │   └── Org_Root\agents.md
│       │
│       ├── Group_A/                                nonce: impl2-grpA ✓
│       │   ├── agents.md chain (2):
│       │   │   ├── Group_A\agents.md
│       │   │   └── Org_Root\agents.md
│       │   │
│       │   ├── GrpA_Team_1/                        nonce: impl2-grpA-team1 ✓
│       │   │   ├── agents.md chain (3):
│       │   │   │   ├── GrpA_Team_1\agents.md
│       │   │   │   ├── Group_A\agents.md
│       │   │   │   └── Org_Root\agents.md
│       │   │   │
│       │   │   └── GrpA_T1_project_1/              nonce: impl2-grpA-team1-proj1 ✓
│       │   │       └── agents.md chain (4):
│       │   │           ├── GrpA_T1_project_1\agents.md
│       │   │           ├── GrpA_Team_1\agents.md
│       │   │           ├── Group_A\agents.md
│       │   │           └── Org_Root\agents.md
│       │   │
│       │   └── GrpA_Team_2/                        nonce: impl2-grpA-team2 ✓
│       │       ├── agents.md chain (3):
│       │       │   ├── GrpA_Team_2\agents.md
│       │       │   ├── Group_A\agents.md
│       │       │   └── Org_Root\agents.md
│       │       │
│       │       └── GrpA_T2_project_1/              nonce: impl2-grpA-team2-proj1 ✓
│       │           └── agents.md chain (4):
│       │               ├── GrpA_T2_project_1\agents.md
│       │               ├── GrpA_Team_2\agents.md
│       │               ├── Group_A\agents.md
│       │               └── Org_Root\agents.md
│       │
│       └── Group_B/                                nonce: impl2-grpB ✓
│           ├── agents.md chain (2):
│           │   ├── Group_B\agents.md
│           │   └── Org_Root\agents.md
│           │
│           ├── GrpB_Team_1/                        nonce: impl2-grpB-team1 ✓
│           │   ├── agents.md chain (3):
│           │   │   ├── GrpB_Team_1\agents.md
│           │   │   ├── Group_B\agents.md
│           │   │   └── Org_Root\agents.md
│           │   │
│           │   └── GrpB_T1_project_1/              nonce: impl2-grpB-team1-proj1 ✓
│           │       └── agents.md chain (4):
│           │           ├── GrpB_T1_project_1\agents.md
│           │           ├── GrpB_Team_1\agents.md
│           │           ├── Group_B\agents.md
│           │           └── Org_Root\agents.md
│           │
│           └── GrpB_Team_2/                        nonce: impl2-grpB-team2 ✓
│               ├── agents.md chain (3):
│               │   ├── GrpB_Team_2\agents.md
│               │   ├── Group_B\agents.md
│               │   └── Org_Root\agents.md
│               │
│               └── GrpB_T2_project_1/              nonce: impl2-grpB-team2-proj1 ✓
│                   └── agents.md chain (4):
│                       ├── GrpB_T2_project_1\agents.md
│                       ├── GrpB_Team_2\agents.md
│                       ├── Group_B\agents.md
│                       └── Org_Root\agents.md
│
└── 3 - environment_variables/
    ├── Location_1/                                 nonce: impl3-location1 ✓
    │   └── agents.md chain (1):
    │       └── 3 - environment_variables\Location_1\agents.md
    │
    ├── Location_2/                                 nonce: impl3-location2 ✓
    │   └── agents.md chain (1):
    │       └── 3 - environment_variables\Location_2\agents.md
    │
    └── Your_actual_project_folder/                 nonce: impl3-project-folder ✓
        └── agents.md chain (1):
            └── 3 - environment_variables\Your_actual_project_folder\agents.md
```

| Impl | Dirs | Max depth | Nonces |
|---|---|---|---|
| 1 — single-folder | 1 | 1 | 1/1 ✓ |
| 2 — multi-folder | 11 | 4 | 11/11 ✓ |
| 3 — env variables | 3 | 1 | 3/3 ✓ |
| **Total** | **15** | — | **15/15 ✓** |

---
sha256: e287ca821ae79155faedb165524c516ab13345cb87576d1d19a95f4afad33d43
