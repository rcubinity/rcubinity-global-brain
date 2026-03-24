# global-brain

Generic **AI coding standards** for Cursor (and similar tools), usable across **many projects** (not tied to one product name). Aligns with common Rcubinity-style patterns: **route catalogs**, **thin controllers**, **service-layer HTTP on Angular**, **colocated context docs**, plus Python services where relevant.

## Contents

| File | Topic |
|------|--------|
| [`.ai/frontend.md`](.ai/frontend.md) | Angular/SPA: signals, smart/dumb split, **named routes / no hardcoded APIs**, models |
| [`.ai/core-angular-skill.md`](.ai/core-angular-skill.md) | Deep Cursor playbook for `@rcubinity/core-angular`: ApiService/ApiCall, coreSignal, auth flow, model mapping |
| [`.ai/backend.md`](.ai/backend.md) | Nest-style: layering, DTOs/validation, **route catalog parity**, guards, errors |
| [`.ai/core-nestjs-skill.md`](.ai/core-nestjs-skill.md) | Deep Cursor playbook for shared Nest `core/*`: decorators, guards, Joi, route metadata, response contract |
| [`.ai/python-services.md`](.ai/python-services.md) | Python: sockets, env config, structure |
| [`.ai/context-and-docs.md`](.ai/context-and-docs.md) | **Atlas**, `*.context.md` capsules, repo overview docs |
| [`.ai/devops.md`](.ai/devops.md) | Docker, CI, env |
| [`.ai/security.md`](.ai/security.md) | Auth, validation, transport |
| [`.ai/testing.md`](.ai/testing.md) | Tests and CI expectations |
| [`.ai/review-checklist.md`](.ai/review-checklist.md) | PR checklist |
| [`.cursorrules`](.cursorrules) | Legacy entry: points at all of the above |

## Usage

### Option A — Add this repo to your Cursor workspace

Open the **global-brain** folder alongside your apps. In each app’s `.cursor/rules/*.mdc`, add **pointers** to the files here (relative or absolute path on disk), e.g. “Canonical frontend API rules: `global-brain/.ai/frontend.md`”.

### Option B — Copy `.ai` into a project

Copy the `.ai` directory into a project root and add a **thin** `.cursor/rules` rule that references `../../path/to/global-brain/.ai/...` or duplicate only what you need.

### Option C — Cursor remote rules

If this repo is on **GitHub**, you can register rules via **Cursor Settings → Rules → Remote (GitHub)** for org-wide defaults, in addition to per-repo `.cursor/rules`.

## Precedence

1. **Project-specific** `.cursor/rules` and local READMEs  
2. **This global-brain** generic guidance  
3. **User rules** in Cursor settings  

When instructions conflict, **follow the target repository’s existing code and rules first.**

## Syncing

Update content here and pull in dependent workspaces, or submodule this repo where your org allows.
