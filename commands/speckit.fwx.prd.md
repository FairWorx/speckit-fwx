---
description: Generate a Product Requirements Document (PRD.md) for a greenfield FairWorx application using the app PRD template
---

<!-- Extension: fwx -->
<!-- Config: .specify/extensions/fwx/ -->

# Generate Application PRD

Produce a `PRD.md` file for a greenfield FairWorx application under `docs/02-agile/`.

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Prerequisites

1. **Source of requirements** — Determine the input source in this order:
   - If `docs/02-agile/requirements.md` exists (from an earlier `/speckit.fwx.requirements` run), read it as the primary source.
   - Otherwise, use the user's text input directly.
   - If neither exists, ask the user to provide requirements or run `/speckit.fwx.requirements` first.

2. **Check project constitution**:
   - **IF EXISTS**: Load `.specify/memory/constitution.md` for project principles and governance constraints.
   - Constitution must be strictly followed.

3. **Read the template**:
   - Load the template from `.specify/extensions/fwx/templates/app_prd_template.md`.

## Workflow

1. **Ask for developer input or confirmation**:
   - Present the inferred application name to the developer for confirmation.
   - Ask about: core feature priorities, data model preferences, API design patterns, UI/UX direction, technical stack choices, and security requirements.
   - Document their responses.

2. **Fill the template**:
   - Replace all `{{PLACEHOLDER}}` values in the template with application-specific content derived from requirements and developer input.
   - Follow these conventions:
     - **Application Overview**: 2-3 paragraphs explaining the application's purpose, target audience, and key outcomes.
     - **Core Features**: Numbered sections (### 1. Title format), each with detailed descriptions, UI behavior, data fields, and business rules. Use tables for status workflows where applicable.
     - **Data Model**: One table per entity with Column, Type, Description columns. Use snake_case naming, standard types (VARCHAR, TEXT, DECIMAL, INTEGER, UUID, TIMESTAMPTZ, DATE, BOOLEAN).
     - **API Architecture**: Table with Method, Endpoint, Description. Endpoints follow `/api/v1/{resource}` pattern.
     - **UI/UX Design**: Describe the overall design direction, layout patterns, component system, and user experience principles.
     - **Technical Architecture**: Describe the system architecture, tech stack choices, hosting, and infrastructure decisions.
     - **Security & Compliance**: Document authentication, authorization, data privacy, and any regulatory requirements.

3. **Output**:
   - Write the filled PRD to `docs/02-agile/PRD.md`.
   - If `docs/02-agile/PRD.md` already exists, present the user with three options:
     - **Replace** — overwrite with newly generated PRD.
     - **Update** — merge new requirements into existing PRD.
     - **Keep existing** — abort generation.

## Template Reference

The template `.specify/extensions/fwx/templates/app_prd_template.md` contains these placeholders:

| Placeholder | Description |
|-------------|-------------|
| `{{APP_NAME}}` | Display name of the application |
| `{{CURRENT_DATE}}` | Today's date in YYYY-MM-DD format |
| `{{APP_OVERVIEW}}` | 2-3 paragraph application overview |
| `{{FEATURE_1_TITLE}}` | Title of first core feature |
| `{{FEATURE_1_DESCRIPTION}}` | Detailed description of first core feature |
| `{{FEATURE_2_TITLE}}` | Title of second core feature |
| `{{FEATURE_2_DESCRIPTION}}` | Detailed description of second core feature |
| `{{FEATURE_3_TITLE}}` | Title of third core feature |
| `{{FEATURE_3_DESCRIPTION}}` | Detailed description of third core feature |
| `{{ENTITY_1_NAME}}` | Primary entity table name |
| `{{FIELD_1}}`, `{{FIELD_1_TYPE}}`, `{{FIELD_1_DESC}}` | Primary entity fields |
| `{{FIELD_2}}`, `{{FIELD_2_TYPE}}`, `{{FIELD_2_DESC}}` | Additional entity fields |
| `{{ENTITY_2_NAME}}` | Secondary entity table name |
| `{{FIELD_3}}`, `{{FIELD_3_TYPE}}`, `{{FIELD_3_DESC}}` | Secondary entity fields |
| `{{FIELD_4}}`, `{{FIELD_4_TYPE}}`, `{{FIELD_4_DESC}}` | Additional entity fields |
| `{{RESOURCE_1}}` | Primary API resource name |
| `{{UI_UX_DIRECTION}}` | UI/UX design direction |
| `{{TECHNICAL_ARCHITECTURE}}` | Technical architecture description |
| `{{SECURITY_COMPLIANCE}}` | Security and compliance requirements |

## PRD Quality Checklist

Ensure the generated PRD follows these rules:
- [ ] Application Overview clearly states purpose, audience, and outcomes
- [ ] Core features are complete with descriptions, UI behavior, and business rules
- [ ] Data model tables follow snake_case naming with Column, Type, Description columns
- [ ] API endpoints use `/api/v1/` prefix
- [ ] UI/UX section describes design direction and principles
- [ ] Technical Architecture section covers stack, hosting, and infrastructure
- [ ] Security & Compliance section addresses auth, authorization, and data privacy
- [ ] Change Log section is included with initial version entry

## References

- **Template file**: `.specify/extensions/fwx/templates/app_prd_template.md`
- **Requirements file**: `docs/02-agile/requirements.md` (if exists)
