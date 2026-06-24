---
description: Clean up and organize raw user input into a structured requirements.md file for a new FairWorx module
---

<!-- Extension: fwx -->
<!-- Config: .specify/extensions/fwx/ -->

# Generate Module Requirements

Take the user's raw module description and produce a structured `requirements.md` file under `docs/02-agile/modules/[module_name]/`.

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Workflow

0. **Check project constitution**:
   - **IF EXISTS**: Load `.specify/memory/constitution.md` for project principles and governance constraints.
   - Constitution must be strictly followed.

1. **Determine module name**:
   - If the user explicitly provided a module name in their input, use it.
   - Otherwise, infer a short snake-case module name from the content with prefix "mod" (e.g., "mod_task_tracker", "mod_expense_reports").
   - Ask the user to confirm the inferred name before proceeding.

2. **Create module directory**:
   - Create the directory `docs/02-agile/modules/[module_name]/`.

3. **Analyze and structure the input**:
   - Read the full user input and reference platform docs for context; always use the latest version of the docs:
     - `docs/02-agile/PRD.md` — understand the main app's architecture, module runtime, Apps tab, access control
     - `docs/02-agile/SDD.md` — understand data model patterns, API endpoint patterns, permissions
     - `docs/02-agile/UI.md` — understand glass-morphism design, layout patterns, component styling, spacing
   - Organize the raw requirements into these sections:
     - **Module Name** — display name for the module
     - **Module Description** — 2-3 sentence summary of what the module does
     - **Core Features** — numbered list of features with clear descriptions
     - **Channel Context** — which space/channel the module belongs to (e.g., FairBase → Finance)
     - **Required Role(s)** — who can access this module (e.g., `member`, `finance_team`, `admin`)
     - **Data Entities** — key database tables needed with columns and types
     - **UI Patterns** — which UI patterns from the design system apply (Kanban, tables, forms, modals, etc.)
     - **Integration Points** — how it integrates with Main App (Apps tab, Files, Chat, Notifications)

4. **Output**:
   - Write the structured content to `docs/02-agile/modules/[module_name]/requirements.md`.
   - Follow markdown formatting with proper headings (##, ###).
   - Use tables where appropriate (matching the existing module documentation style).

## Requirements File Format

```markdown
# Requirements: [Module Display Name]

**Date:** [current date]

## Module Summary

Brief description...

## Core Features

### 1. Feature One
Description of the feature.

### 2. Feature Two
Description of the feature.

## Channel Context

- **Space/Channel**: FairBase → [sub-channel]
- **Required Role(s)**: [role1], [role2]

## Data Entities

### Entity Name
| Field | Type | Description |
|-------|------|-------------|

## UI Patterns

- [Pattern 1]: Description of how it applies
- [Pattern 2]: Description of how it applies

## Integration Points

- **Apps Tab**: ...
- **Files**: ...
- **Notifications**: ...

## Open Questions

- [Any unresolved questions for the developer]
```

## References

Refer to the latest versions of the following files.
- **Platform PRD**: `docs/02-agile/PRD.md`
- **Platform SDD**: `docs/02-agile/SDD.md`
- **UI Design System**: `docs/02-agile/UI.md`

