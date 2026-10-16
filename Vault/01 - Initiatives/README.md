# Initiatives

An **Initiative** represents a major product objective or capability.

## Required structure

```text
01 - Initiatives/
└── Initiative Name/
    ├── PRD.md
    ├── Benchmark.md       # Optional; only when relevant
    └── Slices/
        └── Feature Name.md
```

## Responsibilities

The member responsible for the Jira Epic/major feature owns the initial documentation pass:

1. Create the Initiative folder.
2. Complete `PRD.md`.
3. Add `Benchmark.md` when benchmarking is relevant.
4. Create a Slice overview for each Slice.
5. Create the corresponding canonical Feature folder under `02 - Features/`.
6. Ensure every Slice overview links to exactly one Feature.

Use the templates in `Templates/`.
