---
description: Generate a Product Requirements Document (PRD.md) for a FairWorx module using the module PRD template
---

<!-- Extension: fwx -->
<!-- Config: .specify/extensions/fwx/ -->

# Generate Module PRD

Produce a `PRD.md` file for a FairWorx module under `docs/02-agile/module/[module_name]/`.

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Prerequisites

1. **Source of requirements** — Determine the input source in this order:
   - If `docs/02-agile/module/[module_name]/requirements.md` exists (from an earlier `/speckit.fwx.mod-requirements` run), read it as the primary source.
   - Otherwise, use the user's text input directly.
   - If neither exists, ask the user to provide requirements.

2. **Determine module name**:
   - Check if user explicitly provided a module name.
   - If a `requirements.md` exists, extract the module name from its content.
   - Otherwise, infer a short snake-case module name from the content with prefix "mod" (e.g., "mod_task_tracker", "mod_expense_reports").
   - Ask the user to confirm the inferred name before proceeding.

3. **Check project constitution**:
   - **IF EXISTS**: Load `.specify/memory/constitution.md` for project principles and governance constraints.
   - Constitution must be strictly followed.

4. **Read the template**:
   - Load the template from `.specify/extensions/fwx/templates/module_prd_template.md`.

## Workflow

1. **Gather context** — Read these platform reference docs to ensure the PRD follows platform conventions and always use the latest versions of the docs:
   - `docs/02-agile/PRD.md` — understand the main app's architecture, module runtime, Apps tab behavior, access control, and how module PRDs reference parent documents
   - `docs/02-agile/SDD.md` — understand data model patterns, API endpoint conventions, permissions model, source code organization
   - `docs/02-agile/UI.md` — understand glass-morphism design system, layout patterns (bento grid), component styling, spacing, color system

2. **Ask for developer input or confirmation**:
   - Present the inferred module name, module ID, and typical channel location to the developer for confirmation.
   - Ask about: dependencies on other modules, required roles/permissions, and preferred data model patterns.
   - Document their responses.

3. **Check for existing similar modules**:
   - Search `docs/02-agile/modules/` for existing PRD files whose module name, description, or core features overlap with the current requirements.
   - If one or more existing modules with the same or similar requirements are found, present them to the user and ask for confirmation:
     - **Update existing module** — the current requirements become an update to the existing module's PRD.
     - **Create new module** — proceed with creating a new/modified PRD, ensuring clear differentiation from the existing module(s).
   - Document the user's decision and rationale.

4. **Validate requirements against main platform docs**:
   - Cross-reference the gathered requirements against the three main platform documents and always use the latest versions of the docs:
     - `docs/02-agile/PRD.md` — architecture, module runtime, Apps tab, access control
     - `docs/02-agile/SDD.md` — data model patterns, API conventions, permissions model
     - `docs/02-agile/UI.md` — design system, layout patterns, component styling
   - Identify any requirement that deviates from or conflicts with these main docs.
   - If deviations are found, present them to the user with three options:
     - **Proceed with deviation** — acknowledge the deviation and document the rationale in the PRD.
     - **Update requirements to comply** — adjust the requirements to align with the main docs.
     - **Update main docs** — the new requirement represents a platform-wide change; flag it for inclusion in the next version of the relevant main doc(s).
   - Document the user's decision.

5. **Fill the template**:
   - Replace all `{{PLACEHOLDER}}` values in the template with module-specific content derived from requirements and developer input.
   - Follow these conventions from the example module (`mod_opportunity_pipeline/PRD.md`):
     - **Module Registry table**: Use the same format with ID (e.g., CM5), Module ID (e.g., mod_opportunity_pipeline), Name, Version, Dependencies, Typical Group, Required Role.
     - **Module Description**: 2-3 paragraphs explaining purpose and key outcomes.
     - **Core Features**: Numbered sections (### 1. Title format), each with detailed descriptions, UI behavior, data fields, and business rules. Use tables for status workflows.
     - **Data Model**: One table per entity with Column, Type, Description columns. Follow the same column naming (snake_case, VARCHAR, TEXT, DECIMAL, INTEGER, UUID, TIMESTAMPTZ, DATE, BOOLEAN).
     - **API Endpoints**: Table with Method, Endpoint, Description. Endpoints follow `/api/v1/modules/{resource}` pattern.
     - **Integration with Main App**: Describe Apps Tab placement, Notifications, Files integration, and any cross-module interactions.

6. **Output**:
   - Create the module directory if it doesn't exist: `mkdir -p docs/02-agile/modules/[module_name]/`
   - Write the filled PRD to `docs/02-agile/modules/[module_name]/PRD.md`.

## Template Reference

The template `.specify/extensions/fwx/templates/module_prd_template.md` contains these placeholders:

| Placeholder | Description |
|-------------|-------------|
| `{{MODULE_NAME}}` | Display name of the module |
| `{{MODULE_ID}}` | Module ID code (e.g., CM5) |
| `{{MODULE_CODE}}` | Module code (e.g., mod_opportunity_pipeline) |
| `{{CURRENT_DATE}}` | Today's date in YYYY-MM-DD format |
| `{{DEPENDENCIES}}` | Comma-separated list of module dependencies |
| `{{TYPICAL_GROUP}}` | Channel location (e.g., FairBase → Sales) |
| `{{REQUIRED_ROLE}}` | Comma-separated list of required roles |
| `{{MODULE_DESCRIPTION}}` | 2-3 paragraph module description |
| `{{FEATURE_1_TITLE}}` | Title of first core feature |
| `{{FEATURE_1_DESCRIPTION}}` | Detailed description of first core feature |
| `{{FEATURE_2_TITLE}}` | Title of second core feature |
| `{{FEATURE_2_DESCRIPTION}}` | Detailed description of second core feature |
| `{{FEATURE_3_TITLE}}` | Title of third core feature |
| `{{FEATURE_3_DESCRIPTION}}` | Detailed description of third core feature |
| `{{TABLE_NAME}}` | Primary entity table name |
| `{{FIELD_1}}`, `{{FIELD_1_TYPE}}`, `{{FIELD_1_DESC}}` | Primary entity fields |
| `{{API_BASE}}` | API base path (e.g., `opportunities`) |
| `{{CHANNEL_LOCATION}}` | Where in the channel tree it appears |
| `{{NOTIFICATION_BEHAVIOR}}` | How notifications work |
| `{{FILE_INTEGRATION}}` | How files integrate |
| `{{OTHER_INTEGRATION}}` | Other integration details |

## PRD Quality Checklist

Ensure the generated PRD follows these rules:
- [ ] References parent PRD and SDD documents with correct file paths
- [ ] Has the "must never deviate" warning paragraph
- [ ] Module Registry table is complete
- [ ] Data model tables follow snake_case naming
- [ ] API endpoints use `/api/v1/modules/` prefix
- [ ] UI patterns reference the platform design system (glass-island, bento grid, component patterns from UI v2.md)
- [ ] Follows the same heading structure as the example module PRD
- [ ] Change Log section is included with initial version entry

## References

Always refer to the latest versions of the following docs:
- **Template file**: `.specify/extensions/fwx/templates/module_prd_template.md`
- **Platform PRD**: `docs/02-agile/PRD.md`
- **Platform SDD**: `docs/02-agile/SDD.md`
- **UI Design System**: `docs/02-agile/UI .md`
- **Other module PRDs**: `docs/02-agile/modules/` (review for consistency)
