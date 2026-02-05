# Copilot instructions for this repo

## Scope and structure
- This repository is documentation-only (product workflow + feature specs). There are no build/test commands documented; do not invent any.
- The canonical workflow docs live in Product-Development-Workflow/README.md and the stage files:
  - Intake: Product-Development-Workflow/01-intake.md
  - Define: Product-Development-Workflow/02-define.md
  - Build: Product-Development-Workflow/03-build.md
  - Deliver: Product-Development-Workflow/04-deliver.md
  - Maintain: Product-Development-Workflow/05-maintain.md
- Feature/epic content lives under Features/ and epics/. Preserve existing filenames and folder hierarchy when editing.

## Project-specific patterns to follow
- Preserve the ASCII diagrams, boxed layouts, and section separators used throughout the workflow docs (see Product-Development-Workflow/README.md and 02-define.md). Keep spacing and alignment intact.
- When creating or updating specs, follow the exact spec template in Product-Development-Workflow/02-define.md (two-part spec with “Human-Readable Spec” and “AI-Centric Layer”, no implementation details).
- When creating or updating epics/subtasks, follow the templates and status flow in Product-Development-Workflow/03-build.md (Backlog → To Do → In Progress → Code Review → Ready for Test → Testing → Done / Review Failed / Test Failed).
- For releases, follow the Deliver phase structure in Product-Development-Workflow/04-deliver.md including release ticket, test plan tables, and versioning format vMAJOR.MINOR.PATCH.
- For bugs, follow the Maintain phase template in Product-Development-Workflow/05-maintain.md including severity/SLA and the linked “Fix Task” relationship back to Build.

## Cross-linking conventions
- Requests link to Specs; Specs link to Tasks; Tasks link to Releases; Releases link to Bugs; Bugs link to Fix Tasks. Keep these relation fields consistent with the tables in each stage document.

## Git/branch conventions (documented)
- Use the branch naming formats documented in Product-Development-Workflow/03-build.md:
  - feature/short-description
  - fix/short-description
  - refactor/short-description
  - hotfix/short-description

## Practical editing guidance
- Prefer updating existing templates and examples rather than inventing new formats.
- Use concrete examples already present in the docs for consistency (see the sample Request/Spec/Epic/Subtask/Bug tickets in the workflow files).
