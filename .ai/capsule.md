---
name: capsule
description: After changing source files, update or create the colocated *.context.md capsule so AI and humans keep shared intent. Use when finishing a feature or refactor that alters behavior, routes, or dependencies.
---

# Update capsule

## When to use

- You (or the agent) changed a `.ts` / `.py` file in an IRAS repo in a **substantive** way (behavior, routes, permissions, side effects).

## Steps

1. Locate the sibling capsule: same directory, same basename + `.context.md` (e.g. `user.service.ts` → `user.service.context.md`).
2. If missing, create it using the schema in `workscope/docs/CAPSULES.md` (YAML frontmatter + Purpose / Entry points / Dependencies / Side effects / Invariants).
3. Update `routes` in frontmatter if `default-routes.json` keys or `Request*` `name`/`prefix` changed.
4. Add or fix `related` links to other capsules or Atlas docs.

## Do not

- Duplicate full TypeScript signatures; capsules describe intent and edges, not full typings.

## Reference

- `workscope/docs/CAPSULES.md`
