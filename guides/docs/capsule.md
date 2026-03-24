---
name: capsule
description: Update or create sibling *.context.md after substantive behavior changes.
---

# Update capsule

## Naming

- Source file to capsule: `file.ts` -> `file.context.md` (same directory).
- Repo-level overview can live in a root context doc (for IRAS commonly `IRAS.context.md`).

## Frontmatter

Required:

- `title`
- `layer`
- `module`

Optional:

- `related`
- `routes`

## Steps

1. Locate sibling capsule (`file.ts` -> `file.context.md`).
2. Create if missing using your org capsule schema.
3. Update `routes`/`related` when contracts or dependencies change.
4. Keep intent-level notes; avoid duplicating full type signatures.

## Workflow note

- After substantive behavior changes, update capsule in the same change when possible.
