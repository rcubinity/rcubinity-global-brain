# AI-oriented context and documentation (multi-project)

These practices help humans and coding agents stay aligned across **many repositories** (monorepos or polyrepos).

## Program-level “atlas”

- Maintain **one** high-level map per product line or ecosystem: services, environments, tenancy, and how repos depend on each other (markdown in a **docs** or **meta** repo is enough).
- Link to it from project READMEs or from **Cursor rules** so agents know where architecture lives.

## Colocated context files (capsules)

- For important source files, optionally add a **sibling markdown file** that explains **intent**, **public entry points**, **dependencies**, and **side effects** — not a duplicate of TypeScript types.
- **Naming**: e.g. `feature.service.ts` → `feature.service.context.md` (same directory). Use one convention consistently per org.
- **Frontmatter** (optional): `title`, `layer` (e.g. `angular-service`, `nest-controller`), `module`, `related` links to other capsules or atlas pages.
- **When to update**: whenever behavior, routes, or contracts change in a meaningful way; treat the capsule as **intent documentation**, not generated API reference (use TypeDoc/Swagger for signatures).

## Repository instance overview

- At **repository root**, a short `README` or a single **instance overview** file (e.g. `INSTANCE.context.md`) can describe what **this repo** is responsible for in the larger system.

## Automation

- Scripts can **index** all `*.context.md` files into a **manifest** for a static doc site; they do not replace thoughtful writing.
- **Stubs** can be bulk-generated for coverage; teams should **trim or deepen** stubs on critical paths.

## Precedence

- **Project-specific** `.cursor/rules` and **local conventions** override these generic notes when they conflict.
