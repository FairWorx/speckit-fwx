---
description: Generate a Solution Design Document (SDD.md) for a greenfield FairWorx application using the app SDD template
---

<!-- Extension: fwx -->
<!-- Config: .specify/extensions/fwx/ -->

# Generate Application SDD

Produce a `SDD.md` file for a greenfield FairWorx application under `docs/02-agile/`.

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Prerequisites

1. **Source of requirements** — Determine the input source in this order:
   - If `docs/02-agile/PRD.md` exists, read it as the primary source (PRD contains the feature descriptions and data model requirements).
   - Otherwise, if `docs/02-agile/requirements.md` exists (from an earlier `/speckit.fwx.requirements` run), read it.
   - Otherwise, use the user's text input directly.
   - If none exist, ask the user to provide requirements, run `/speckit.fwx.requirements`, or create a PRD first via `/speckit.fwx.prd`.

2. **Check project constitution**:
   - **IF EXISTS**: Load `.specify/memory/constitution.md` for project principles and governance constraints.
   - Constitution must be strictly followed.

3. **Read the template**:
   - Load the template from `.specify/extensions/fwx/templates/app_sdd_template.md`.

## Workflow

1. **Read existing PRD for the application**:
   - Read `docs/02-agile/PRD.md` thoroughly to extract:
     - Core features and their detailed descriptions
     - Data entity definitions (tables, columns, types)
     - API endpoint specifications
     - UI/UX direction
     - Technical architecture guidance
   - If no PRD exists, read the `requirements.md` or user input and note that an SDD is being created without a formal PRD.

2. **Ask for developer input or confirmation**:
   - Present the inferred application name to the developer for confirmation.
   - Ask about: tech stack choices (backend language, frontend framework, database, infrastructure), data model preferences, API design patterns, frontend component architecture, and deployment strategy.
   - Document their responses.

3. **Fill the template**:
   - Replace all `{{PLACEHOLDER}}` values in the template with application-specific content derived from the PRD/requirements and developer input.
   - Follow these conventions:
     - **System Overview**: 2-3 paragraphs explaining the system architecture and how components interact.
     - **Data Models**: One table per entity with Column, Type, Notes columns. Column names in snake_case. Include Alembic migration code blocks showing `op.create_table()` / `op.drop_table()` for each entity.
     - **API Design**: Table with Method, Path, Description. Paths follow `/api/v1/{resource}` pattern. Include request/response schemas with validation rules.
     - **Frontend Architecture**: Show file structure tree, component tree, UI patterns, and states (Loading, Error, Empty).
     - **Backend Architecture**: Describe the backend structure, service layer, business logic, background tasks, and middleware.
     - **Deployment**: Describe hosting, CI/CD, containerization, environment configuration, and scaling strategy.

4. **Output**:
   - Write the filled SDD to `docs/02-agile/SDD.md`.
   - If `docs/02-agile/SDD.md` already exists, present the user with three options:
     - **Replace** — overwrite with newly generated SDD.
     - **Update** — merge new design into existing SDD.
     - **Keep existing** — abort generation.

## Template Reference

The template `.specify/extensions/fwx/templates/app_sdd_template.md` contains these placeholders:

| Placeholder | Description |
|-------------|-------------|
| `{{APP_NAME}}` | Display name of the application |
| `{{CURRENT_DATE}}` | Today's date in YYYY-MM-DD format |
| `{{SYSTEM_OVERVIEW}}` | 2-3 paragraph system architecture overview |
| `{{BACKEND_TECH}}` | Backend tech stack details |
| `{{FRONTEND_TECH}}` | Frontend tech stack details |
| `{{DATABASE_TECH}}` | Database tech stack details |
| `{{INFRA_TECH}}` | Infrastructure tech stack details |
| `{{ENTITY_1_NAME}}` | Primary entity table name |
| `{{ENTITY_1_COL_1}}`, `{{ENTITY_1_TYPE_1}}`, `{{ENTITY_1_DESC_1}}` | Primary entity field definitions |
| `{{ENTITY_1_COL_2}}`, `{{ENTITY_1_TYPE_2}}`, `{{ENTITY_1_DESC_2}}` | Additional entity field definitions |
| `{{ENTITY_2_NAME}}` | Secondary entity table name |
| `{{ENTITY_2_COL_1}}`, `{{ENTITY_2_TYPE_1}}`, `{{ENTITY_2_DESC_1}}` | Secondary entity field definitions |
| `{{ENTITY_2_COL_2}}`, `{{ENTITY_2_TYPE_2}}`, `{{ENTITY_2_DESC_2}}` | Additional entity field definitions |
| `{{MIGRATION_FILE}}` | Alembic migration filename |
| `{{API_PATH_1}}` | Primary API resource path (e.g., `/users`) |
| `{{FRONTEND_ENTRY}}` | Frontend entry point |
| `{{FRONTEND_STRUCTURE}}` | Frontend directory tree |
| `{{COMPONENT_TREE}}` | Component hierarchy |
| `{{UI_PATTERN_1}}` | First UI pattern used |
| `{{UI_PATTERN_2}}` | Second UI pattern used |
| `{{LOADING_STATE}}` | Loading state description |
| `{{ERROR_STATE}}` | Error state description |
| `{{EMPTY_STATE}}` | Empty state description |
| `{{BACKEND_ARCHITECTURE}}` | Backend architecture description |
| `{{DEPLOYMENT_DETAILS}}` | Deployment and infrastructure details |

## SDD Quality Checklist

Ensure the generated SDD follows these rules:
- [ ] References the PRD document with correct file path
- [ ] System Overview clearly explains architecture and component interactions
- [ ] Data model tables follow snake_case naming with Column, Type, Notes columns
- [ ] Alembic migration code included with `upgrade()` and `downgrade()` functions
- [ ] API endpoints use `/api/v1/` prefix
- [ ] Frontend architecture includes file structure, component tree, UI patterns, and states
- [ ] Backend architecture describes service layer, business logic, and middleware
- [ ] Deployment section covers hosting, CI/CD, and infrastructure
- [ ] Tech Stack table is complete for all layers
- [ ] Change Log section is included with initial version entry

## References

- **Template file**: `.specify/extensions/fwx/templates/app_sdd_template.md`
- **Application PRD**: `docs/02-agile/PRD.md`
- **Requirements file**: `docs/02-agile/requirements.md` (if exists)
