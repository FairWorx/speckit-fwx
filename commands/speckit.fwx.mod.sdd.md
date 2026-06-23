---
description: Generate a Solution Design Document (SDD.md) for a FairWorx module using the module SDD template
---

<!-- Extension: fwx -->
<!-- Config: .specify/extensions/fwx/ -->

# Generate Module SDD

Produce a `SDD.md` file for a FairWorx module under `docs/02-agile/modules/[module_name]/`.

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Prerequisites

1. **Determine module name**:
   - Check if user explicitly provided a module name or a PRD.md file
   - If a `PRD.md` exists, extract the module name
   - Otherwise, infer a short snake-case module name with prefix "mod" and ask for confirmation.

2. **Source of requirements** — Determine the input source in this order:
   - If `docs/02-agile/modules/[module_name]/PRD.md` exists, read it as the primary source (PRD contains the feature descriptions and data model requirements).
   - Otherwise, if `docs/02-agile/module/[module_name]/requirements.md` exists (from an earlier `/speckit.fwx.mod.requirements` run), read it.
   - Otherwise, use the user's text input directly.
   - If none exist, ask the user to provide requirements or create a PRD first.

3. **Check project constitution**:
   - **IF EXISTS**: Load `.specify/memory/constitution.md` for project principles and governance constraints.
   - Constitution must be strictly followed.

4. **Read the template**:
   - Load the template from `.specify/extensions/fwx/templates/module_sdd_template.md`.

## Workflow

1. **Gather context** — Read these platform reference docs to ensure the SDD follows platform conventions and always use the latest versions:
   - `docs/02-agile/PRD.md` — understand the main app's architecture, module runtime, Apps tab behavior, access control
   - `docs/02-agile/SDD.md` — understand data model patterns, API endpoint conventions, permissions model, source code organization
   - `docs/02-agile/UI.md` — understand glass-morphism design system, layout patterns, component styling

2. **Read existing PRD for the module**:
   - Read `docs/02-agile/modules/[module_name]/PRD.md` thoroughly to extract:
     - Core features and their detailed descriptions
     - Data entity definitions (tables, columns, types)
     - API endpoint specifications
     - UI patterns and component requirements
     - Integration points with the Main App
   - If no PRD exists, read the `requirements.md` or user input and note that an SDD is being created without a formal PRD.

3. **Ask for developer input or confirmation**:
   - Present the inferred module name, module ID, and module code to the developer for confirmation.
   - Ask about: tech stack choices (backend language, frontend framework, database), data model preferences, API design patterns, frontend component architecture, and any module manifest details.
   - Document their responses.

4. **Check for existing similar modules**:
   - Search `docs/02-agile/modules/` for existing SDD files whose module name, description, or data models overlap with the current module.
   - If one or more existing modules with the same or similar design are found, present them to the user and ask for confirmation:
     - **Update existing module** — the current design becomes an update to the existing module's SDD.
     - **Create new module** — proceed with creating a new SDD, ensuring clear differentiation from the existing module(s).
   - Document the user's decision and rationale.

5. **Validate design against main platform docs**:
   - Cross-reference the design decisions against the three main platform documents and always use the latest versions:
     - `docs/02-agile/PRD.md` — verify features align with architecture
     - `docs/02-agile/SDD.md` — verify data model patterns, API conventions, permissions model
     - `docs/02-agile/UI.md` — verify UI patterns align with design system
   - Identify any design that deviates from or conflicts with these main docs.
   - If deviations are found, present them to the user with three options:
     - **Proceed with deviation** — acknowledge the deviation and document the rationale in the SDD.
     - **Update design to comply** — adjust the design to align with the main docs.
     - **Update main docs** — the new design represents a platform-wide change; flag it for inclusion in the next version of the relevant main doc(s).
   - Document the user's decision.

6. **Fill the template**:
   - Replace all `{{PLACEHOLDER}}` values in the template with module-specific content derived from the PRD/requirements and developer input.
   - Follow these conventions from existing module SDDs:
     - **Module Registry table**: Use the same format with ID, Module ID, Name, Version, Dependencies, Typical Group, Required Role (match the PRD exactly).
     - **Data Models**: One table per entity with Column, Type, Notes columns. Column names in snake_case. Include Alembic migration code blocks showing `op.create_table()` / `op.drop_table()` for each entity.
     - **API Endpoints**: Table with Method, Path, Description. Paths follow `/api/v1/modules/{resource}` pattern. Include route prefix.
     - **API Schemas**: Include Pydantic model definitions (for Python backend) showing `OpportunityCreate`, `OpportunityUpdate`, `OpportunityResponse`, etc. with full field definitions and validation rules.
     - **API Endpoint implementations**: Include FastAPI route definitions (or equivalent for other frameworks) with `@router.get/post/patch/delete`, dependency injection for auth/permissions, error handling, and response models.
     - **Frontend Component**: Show file structure tree, component tree, UI patterns used (ResponsiveModal, glass-island, bento grid), and states (Loading, Error, Empty, Saving).
     - **Service Layer**: Include business logic details like status transition rules, ID generation, computed fields.
     - **Module Manifest**: JSON block with `module_id`, `name`, `description`, `version`, `required_permissions`, `frontend_entry`, `has_unsaved_check`, `dependencies`.
   - When a module has no custom database tables (e.g., uses existing Files API), note this explicitly and describe the integration point instead.
   - When a module is simple, omit sections that are not applicable (e.g., no service layer or no background tasks) rather than leaving them empty.

7. **Output**:
   - Create the module directory if it doesn't exist: `mkdir -p docs/02-agile/modules/[module_name]/`
   - Write the filled SDD to `docs/02-agile/modules/[module_name]/SDD.md`.

## Template Reference

The template `.specify/extensions/fwx/templates/module_sdd_template.md` contains these placeholders:

| Placeholder | Description |
|-------------|-------------|
| `{{MODULE_NAME}}` | Display name of the module |
| `{{MODULE_ID}}` | Module ID code (e.g., C4) |
| `{{MODULE_CODE}}` | Module code (e.g., mod_quick_notes) |
| `{{CURRENT_DATE}}` | Today's date in YYYY-MM-DD format |
| `{{DEPENDENCIES}}` | Comma-separated list of module dependencies |
| `{{TYPICAL_GROUP}}` | Channel location (e.g., FairBase → Sales) |
| `{{REQUIRED_ROLE}}` | Comma-separated list of required roles |
| `{{MODULE_OVERVIEW}}` | 1-2 sentence summary of what the module implements |
| `{{BACKEND_TECH}}` | Backend tech stack details |
| `{{FRONTEND_TECH}}` | Frontend tech stack details |
| `{{DATABASE_TECH}}` | Database tech stack details |
| `{{TABLE_1_NAME}}` | Primary entity table name |
| `{{TABLE_1_COL_1}}`, `{{TABLE_1_TYPE_1}}`, `{{TABLE_1_DESC_1}}` | Primary entity field definitions |
| `{{TABLE_1_COL_2}}`, `{{TABLE_1_TYPE_2}}`, `{{TABLE_1_DESC_2}}` | Additional entity field definitions |
| `{{MIGRATION_FILE}}` | Alembic migration filename |
| `{{API_PREFIX}}` | API route prefix (e.g., `/api/v1/modules/quick-notes`) |
| `{{UI_PATTERN_1}}` | First UI pattern used |
| `{{UI_PATTERN_2}}` | Second UI pattern used |
| `{{LOADING_STATE}}` | Loading state description |
| `{{ERROR_STATE}}` | Error state description |
| `{{EMPTY_STATE}}` | Empty state description |
| `{{MODULE_DESCRIPTION_SHORT}}` | Short description for manifest |
| `{{MANIFEST_PERMISSIONS}}` | JSON array of required permissions |
| `{{FRONTEND_ENTRY}}` | Frontend entry point path |
| `{{HAS_UNSAVED_CHECK}}` | Boolean for unsaved changes check |
| `{{MANIFEST_DEPENDENCIES}}` | JSON array of module dependencies |
| `{{CHANNEL_LOCATION}}` | Where in the channel tree it appears |
| `{{PERMISSION_DETAILS}}` | Permission guard details |
| `{{OTHER_INTEGRATION}}` | Other integration details |

## SDD Quality Checklist

Ensure the generated SDD follows these rules:
- [ ] References parent SDD and PRD documents with correct file paths
- [ ] Has the "must never deviate" warning paragraph
- [ ] Module Registry table matches the PRD exactly
- [ ] Data model tables follow snake_case naming with Column, Type, Notes columns
- [ ] Alembic migration code included with `upgrade()` and `downgrade()` functions
- [ ] API endpoints use `/api/v1/modules/` prefix
- [ ] API schemas include Create, Update, Response Pydantic models with validation
- [ ] API endpoint implementations show FastAPI route definitions with auth/permission guards
- [ ] Frontend component structure includes file tree, component tree, UI patterns, and states
- [ ] Service layer included when there is non-trivial business logic (status transitions, computed fields, ID generation)
- [ ] Module manifest JSON is complete
- [ ] UI patterns reference the platform design system (glass-island, bento grid, ResponsiveModal)
- [ ] Follows the same heading structure as existing module SDDs
- [ ] Change Log section is included with initial version entry

## References

Always refer to the latest versions of the following docs:
- **Template file**: `.specify/extensions/fwx/templates/module_sdd_template.md`
- **Template file for PRD**: `.specify/extensions/fwx/templates/module_prd_template.md`
- **Platform PRD**: `docs/02-agile/PRD.md`
- **Platform SDD**: `docs/02-agile/SDD.md`
- **UI Design System**: `docs/02-agile/UI.md`
- **Existing module SDDs**: `docs/02-agile/modules/` (review for consistency)
- **PRD command reference**: `.specify/extensions/fwx/commands/speckit.fwx.mod.prd.md` (follow same procedural patterns)
