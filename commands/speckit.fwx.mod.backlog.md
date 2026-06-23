---
description: Generate a module-level BACKLOG.md by breaking down the PRD into epics and Product Backlog Items (PBIs)
---

<!-- Extension: fwx -->
<!-- Config: .specify/extensions/fwx/ -->

# Generate Module Backlog

Produce a `BACKLOG.md` file for a FairWorx module under `docs/02-agile/modules/[module_name]/`.

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Prerequisites

1. **PRD must exist** — A `PRD.md` file for the module is required. Determine the source in this order:
   - If the user provided a path or module name in the arguments, use it.
   - Otherwise, search `docs/02-agile/modules/*/PRD.md` and infer the module name from the `$ARGUMENTS` content.
   - Ask the user to confirm the inferred module name before proceeding.
   - If no PRD is found, ask the user to create one first via `/speckit.fwx.mod.prd`.

2. **Determine module directory**:
   - Once the module name is confirmed, set `module_dir = docs/02-agile/modules/[module_name]/`.
   - Verify `PRD.md` exists at `[module_dir]/PRD.md`.
   - Check if `[module_dir]/requirements.md` exists (from an earlier `/speckit.fwx.mod.requirements` run) for additional detail.

3. **Read the template**:
   - Load the template from `.specify/extensions/fwx/templates/module_backlog_template.md`.

4. **Check for existing BACKLOG**:
   - If `[module_dir]/BACKLOG.md` already exists, present the user with three options:
     - **Replace** — overwrite with newly generated backlog.
     - **Update** — merge new PBIs into existing, preserving completed status.
     - **Keep existing** — abort generation.
   - Document the user's decision.

## Workflow

1. **Gather context** — Read these platform reference docs to understand how the module fits into the broader system and always use the latest versions of the docs:
   - `docs/02-agile/PRD.md` — understand the main app's architecture, module runtime, Apps tab, access control, and the specific section(s) covering this module.
   - `docs/02-agile/SDD.md` — understand data model patterns, API endpoint conventions, permissions model, source code organization.
   - `docs/02-agile/UI.md` — understand glass-morphism design system, layout patterns, component styling.
   - The module's `PRD.md` at `[module_dir]/PRD.md` — extract Module Registry (ID, name, version, dependencies, typical group, required role) and Module Description (core purpose, features, outcomes).
   - If `[module_dir]/requirements.md` exists, read it for additional feature details, data entities, UI patterns, and integration points.

2. **Extract and organize module features**:
   - From the module PRD description, parent PRD/SDD sections, and requirements.md, compile a complete list of features/functionality the module must deliver.
   - For each feature, identify:
     - **Description** — what it does
     - **Dependencies** — which other features or modules it depends on
     - **Priority** — P0 (Critical — must have for launch), P1 (High — important), P2 (Medium — nice to have), P3 (Low — future)
     - **Effort estimate** — rough: Small / Medium / Large
     - **Functional role** — who uses it (e.g., member, finance_team, admin)

3. **Ask for developer input or confirmation**:
   - Present the extracted feature list to the developer for confirmation.
   - Ask the developer to:
     - Confirm or adjust the feature list and descriptions.
     - Suggest a logical grouping of features into Epics.
     - Confirm priority levels and ordering (foundational items must come first).
     - Identify any missing features.
   - Document their responses.

4. **Group features into Epics**:
   - Organize related features into logical Epics. Each Epic should represent a cohesive slice of functionality.
   - Guidelines for Epic grouping:
     - **Phase-based** — e.g., "Foundation" (DB, models, API), "Core Features", "Advanced Features"
     - **Feature-area-based** — e.g., "User Management", "Reporting", "Notifications"
     - **CRUD-based** — e.g., "Entity A CRUD", "Entity B CRUD", "Cross-Entity Workflows"
   - Name each Epic descriptively.
   - If there are features that cannot be grouped into any named Epic, place them in a "Future Backlog" section.

5. **Order Epics and PBIs properly**:
   - Arrange Epics so that foundational work (data models, infrastructure, API scaffolding) appears first.
   - Within each Epic, order PBIs so that:
     - Database migrations come before API endpoints.
     - API endpoints come before frontend components.
     - Read operations come before write operations where sensible.
     - Simple CRUD comes before complex business logic.
   - Ensure no Epic or PBI has a dependency on a later-ordered PBI or Epic.
   - Flag any circular or conflicting dependencies to the developer for resolution.

6. **Break Epics into PBIs**:
   - Decompose each Epic into individual Product Backlog Items (PBIs).
   - Each PBI must be:
     - **Small enough** to complete in a sprint (1-5 days of work).
     - **Independent** — minimises dependencies on other in-flight PBIs.
     - **Valuable** — delivers a piece of working functionality on its own.
     - **Testable** — has clear acceptance criteria.
   - For each PBI, generate:

     #### a. PBI Numbering
     - Format: `PBI-{Epic#}-{seq}` where:
       - `Epic#` = the Epic's sequential number within the backlog.
       - `seq` = 3-digit zero-padded sequence within the Epic (001, 002, ...).
     - Example: `PBI-12-001`, `PBI-12-002` for Epic 12.

     #### b. Priority
     - Use a single priority level from the PRD/input: P0 (Critical), P1 (High), P2 (Medium), P3 (Low).

     #### c. User Story
     - Format: `As a **[Role]**, [user story].`
     - Derive the role from the module's required role or the specific feature context.

     #### d. Acceptance Criteria (Functional)
     - Write clear, testable acceptance criteria as a checklist.
     - Each AC must be verifiable (pass/fail).
     - Append a **test type annotation** in square brackets after each AC:
       - `[unit]` — can and should be tested via unit tests (pure logic, isolated).
       - `[integration]` — requires integration testing (DB, API, external service).
       - `[e2e]` — end-to-end / browser test (Playwright, Cypress).
       - `[human]` — must be verified manually by a human (UI review, UX flow, stakeholder sign-off).
     - Example:
       ```
       - [] GET endpoint returns 200 with paginated results [integration]
       - [] Empty state shows "No items found" placeholder [e2e]
       - [] Business rule: status transition Pending → Approved requires admin role [unit]
       - [] UI matches the mockup in Figma [human]
       ```

     #### e. Technical Guidance
     - Include relevant technical notes: which files to modify, patterns to follow, existing services to reuse, migration details.

     #### f. Constraints (Non-Functional) if any
     - Performance requirements, security constraints, accessibility, browser support, etc.

7. **Create the Requirements Traceability Matrix**:
   - After all PBIs are listed, insert a section titled `## Requirements Traceability Matrix`.
   - Build a table mapping every requirement from the PRD (and requirements.md if available) to the PBIs that implement it.
   - Columns: `PRD Requirement | PBI(s) | Coverage`
   - If the PRD uses numbered features, reference them by number. Otherwise, reference by logical feature name.
   - If the same requirement spans multiple PBIs, list all of them.
   - Mark any PRD requirement not covered by any PBI as `⚠️ NOT COVERED` — must be resolved with the developer.

8. **Fill the template**:
   - Replace all `{{PLACEHOLDER}}` values in the backlog template with module-specific content derived from the analysis above.
   - Follow these conventions from the existing product backlog (`docs/02-agile/BACKLOG.md`):
     - **Backlog Status Summary**: Count PBIs by status (all start as Todo for new backlogs).
     - **Epic structure**: Each Epic gets a header with `**Status:**`, `**PBIs:**` summary, then individual PBI subsections.
     - **PBI format**: Full PBIs use the complete template (Priority, References, User Story, Acceptance Criteria, Technical Guidance, Constraints). Simple/obvious PBIs may use a condensed inline format.
     - **References**: Reference the parent PRD section (e.g., `PRD v2.0 §6.1 (F1)`), the module PRD, and any relevant SDD sections.
     - **Dependency Graph**: Optionally include an ASCII dependency tree at the end of the document, following the pattern from BACKLOG-v2.10.md.

9. **Output**:
   - Create the module directory if it doesn't exist: `mkdir -p [module_dir]`.
   - Write the filled backlog to `[module_dir]/BACKLOG.md`.

## Template Reference

The template `.specify/extensions/fwx/templates/module_backlog_template.md` contains these placeholders:

| Placeholder | Description |
|-------------|-------------|
| `{{MODULE_NAME}}` | Display name of the module (e.g., "Group Dashboard", "Member Dues") |
| `{{MODULE_CODE}}` | Module code (e.g., C1, F1) |
| `{{MODULE_ID}}` | Module ID (e.g., mod_group_dashboard, mod_member_dues) |
| `{{VERSION}}` | Version from PRD |
| `{{CURRENT_DATE}}` | Today's date in YYYY-MM-DD format |
| `{{TODO_COUNT}}` | Number of Todo PBIs |
| `{{IN_PROGRESS_COUNT}}` | Number of In Progress PBIs (always 0 for new backlogs) |
| `{{DONE_COUNT}}` | Number of Done PBIs (always 0 for new backlogs) |
| `{{TOTAL_COUNT}}` | Total number of PBIs |
| `{{EPIC_1_TITLE}}` | Title of first Epic |
| `{{EPIC_1_PBIS}}` | PBI range and summary for first Epic |
| `{{EPIC_N_TITLE}}` | Title of Nth Epic |
| `{{EPIC_N_PBIS}}` | PBI range and summary for Nth Epic |
| `{{PBI_LIST}}` | All generated PBIs across all Epics (epic headers + PBI entries) |
| `{{TRACEABILITY_MATRIX}}` | The requirements traceability table |
| `{{RELATED_DOCS}}` | Links to related documents |
| `{{CHANGE_LOG}}` | Initial change log entry |

## Backlog Quality Checklist

Ensure the generated BACKLOG.md follows these rules:
- [ ] Module name, ID, and code match the PRD exactly
- [ ] Every requirement from the PRD is covered by at least one PBI
- [ ] PBIs are grouped into logical Epics with descriptive names
- [ ] PBI numbering follows `PBI-{Epic#}-{seq}` format consistently
- [ ] Epics are ordered so foundational work comes first
- [ ] Within each Epic, PBIs respect dependency order (migrations → API → frontend)
- [ ] No conflicting priorities within an Epic
- [ ] Each acceptance criteria has a test type annotation: [unit], [integration], [e2e], or [human]
- [ ] Requirements Traceability Matrix is complete with no "NOT COVERED" entries
- [ ] User stories follow the "As a **[Role]**" format
- [ ] References to parent PRD/SDD use the correct file paths and section numbers
- [ ] Change Log section is included with initial version entry
- [ ] Related Documents section links to PRD and SDD
- [ ] The generated backlog follows the same structural conventions as `docs/02-agile/BACKLOG-v2.10.md`

## References

Always refer to the latest versions of the following docs:
- **Template file**: `.specify/extensions/fwx/templates/module_backlog_template.md`
- **Platform PRD**: `docs/02-agile/PRD.md`
- **Platform SDD**: `docs/02-agile/SDD.md`
- **UI Design System**: `docs/02-agile/UI.md`
- **Existing product backlog**: `docs/02-agile/BACKLOG.md` (review for PBI format and numbering conventions)
- **Module PRD**: `docs/02-agile/modules/[module_name]/PRD.md`
- **Module requirements**: `docs/02-agile/modules/[module_name]/requirements.md` (if exists)
- **Other module backlogs**: `docs/02-agile/modules/*/BACKLOG.md` (review for consistency)
