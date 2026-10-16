# Features

This is the **canonical home** for full feature documentation. Each feature has its own folder.

## Canonical structure

```text
02 - Features/
└── Feature Name/
    ├── Overview.md
    ├── Requirements.md
    ├── Technical Design.md
    ├── Testing.md
    └── Tasks/
        └── JIRA-123.md
```

## Responsibilities

- Each Slice under `01 - Initiatives/<Initiative Name>/Slices/` corresponds to exactly one Feature here.
- The Slice overview lives inside its Initiative's `Slices/` folder; the detailed documentation lives here. Avoid maintaining duplicate copies.
- `Overview.md`, `Requirements.md`, `Technical Design.md`, and `Testing.md` contain canonical, durable knowledge about the feature.
- `Tasks/` contains execution records for individual Jira tasks (e.g. `Tasks/JIRA-123.md`), recording grilling decisions, approved plans, test verification evidence, and lifecycle stage completions.
- Task records do not replace canonical feature documents, and canonical documents do not duplicate per-task execution logs.

Use the templates in `Templates/`. Update the existing feature documents after implementation rather than creating a competing “final” copy.
