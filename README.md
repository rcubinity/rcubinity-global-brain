# global-brain

Generic **AI coding standards** for Cursor (and similar tools), usable across **many projects** (not tied to one product name). Aligns with common Rcubinity-style patterns: **route catalogs**, **thin controllers**, **service-layer HTTP on Angular**, **colocated context docs**, plus Python services where relevant.

## Contents (framework + design taxonomy)

| File | Topic |
|------|--------|
| [`guides/frontend/angular/rules.md`](guides/frontend/angular/rules.md) | Angular frontend baseline |
| [`guides/frontend/angular/core-angular-skill.md`](guides/frontend/angular/core-angular-skill.md) | Deep `@rcubinity/core-angular` skill |
| [`guides/backend/nestjs/rules.md`](guides/backend/nestjs/rules.md) | NestJS backend baseline |
| [`guides/backend/nestjs/core-nestjs-skill.md`](guides/backend/nestjs/core-nestjs-skill.md) | Deep `core/*` NestJS skill |
| [`guides/backend/python/rules.md`](guides/backend/python/rules.md) | Python services/workers baseline |
| [`guides/design/ui-ux-styling-bootstrap/ui.md`](guides/design/ui-ux-styling-bootstrap/ui.md) | UI guidance |
| [`guides/design/ui-ux-styling-bootstrap/ux.md`](guides/design/ui-ux-styling-bootstrap/ux.md) | UX guidance |
| [`guides/design/ui-ux-styling-bootstrap/styling.md`](guides/design/ui-ux-styling-bootstrap/styling.md) | Styling guidance |
| [`guides/design/ui-ux-styling-bootstrap/bootstrap.md`](guides/design/ui-ux-styling-bootstrap/bootstrap.md) | Bootstrap guidance |
| [`guides/platform/devops.md`](guides/platform/devops.md) | DevOps and delivery |
| [`guides/platform/security.md`](guides/platform/security.md) | Security baseline |
| [`guides/platform/testing.md`](guides/platform/testing.md) | Testing expectations |
| [`guides/platform/review-checklist.md`](guides/platform/review-checklist.md) | Review checklist |
| [`guides/docs/context-and-docs.md`](guides/docs/context-and-docs.md) | Atlas/context-doc conventions |
| [`guides/docs/capsule.md`](guides/docs/capsule.md) | Capsule update workflow |
| [`.cursorrules`](.cursorrules) | Root pointer to all of the above |

## Usage

### Option A — Add this repo to your Cursor workspace

Open the **global-brain** folder alongside your apps. In each app’s `.cursor/rules/*.mdc`, add pointers to these paths, e.g. `global-brain/guides/frontend/angular/rules.md`.

### Option B — Copy `guides` into a project

Copy the `guides` directory into a project root and add a thin `.cursor/rules` rule that references `../../path/to/global-brain/guides/...`.

### Option C — Cursor remote rules

If this repo is on **GitHub**, you can register rules via **Cursor Settings → Rules → Remote (GitHub)** for org-wide defaults, in addition to per-repo `.cursor/rules`.

## Precedence

1. **Project-specific** `.cursor/rules` and local READMEs  
2. **This global-brain** generic guidance  
3. **User rules** in Cursor settings  

When instructions conflict, **follow the target repository’s existing code and rules first.**

## Syncing

Update content here and pull in dependent workspaces, or submodule this repo where your org allows.
