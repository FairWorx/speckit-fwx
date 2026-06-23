---
description: Generate sprint report, devlog, update as-is docs, and sync SDD after implementation
---

<!-- Extension: fwx -->
<!-- Config: .specify/extensions/fwx/ -->

# Generate Sprint Docs & Sync SDD

Produce sprint documentation (as-is doc updates, sprint report, devlog) and update the module SDD to reflect any implementation changes. This command consolidates Steps 13-15 of the developer workflow.

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Prerequisites

1. **Determine module name**:
   - If the user explicitly provided a module name or path in the arguments, use it.
   - Otherwise, look for the most recently modified module directory in `docs/02-agile/modules/` that has both `PRD.md` and `SDD.md`.
   - Ask the user to confirm the inferred module name before proceeding.

2. **Determine sprint number**:
   - Scan `docs/03-sprints/` for existing sprint reports. Find the highest sprint number (e.g., sprint-030-report.md → 30).
   - The new sprint number is `highest + 1`.
   - If no sprint reports exist, start at sprint 1.
   - Ask the user to confirm the sprint number.

3. **Read existing reference docs and templates** — Read these to understand the current state of the platform and always use the latest versions:
   - `docs/01-as-is/business-context-actual.md` — current business capabilities
   - `docs/01-as-is/system-map.md` — current architecture and API endpoints
   - `docs/01-as-is/data-schema-actual.md` — current database schema
   - `docs/01-as-is/technical-debt.md` — current technical debt items
   - `templates/business_context_template.md` — template for business context docs
   - `templates/system_map_template.md` — template for system map docs
   - `templates/data_schema_template.md` — template for data schema docs
   - `templates/technical_debt_template.md` — template for technical debt docs
   - `templates/sprint_report_template.md` — template for sprint reports
   - The module's `PRD.md` at `docs/02-agile/modules/[module_name]/PRD.md` — planned requirements
   - The module's `SDD.md` at `docs/02-agile/modules/[module_name]/SDD.md` — planned design
   - The spec file `specs/*/[module_name]/spec.md` or the most recent spec directory — original specification
   - The most recent sprint report in `docs/03-sprints/` for format reference
   - The most recent devlog in `docs/06-devlog/` for format reference

4. **Check for speckit artifacts** — Read any available speckit artifacts to understand what was implemented:
   - `specs/*/plan.md` — implementation plan (original design intent)
   - `specs/*/tasks.md` — task list (what was supposed to be implemented)
   - `specs/*/research.md` — research output
   - `specs/*/data-model.md` — planned data model changes
   - `specs/*/contracts/` — API contract definitions
   - `specs/*/quickstart.md` — quickstart guide with implementation details
   - `specs/*/spec.md` — the original feature specification

5. **Scan git diff for actual changes** — Run `git diff dev...HEAD --stat` and `git diff dev...HEAD` to identify all files that were modified, added, or deleted during this session. This is the ground truth for what changed. Classify changes into:
   - **Backend**: Python files (api/, models/, schemas/, services/, tasks/, alembic/, modules/)
   - **Frontend**: TypeScript/React files (components/, pages/, stores/, hooks/, lib/)
   - **Tests**: test files (backend/tests/, frontend/tests/)
   - **Docs**: documentation files (docs/)
   - **Config**: configuration files (docker/, package.json, pyproject.toml, etc.)

6. **Check project constitution**:
   - **IF EXISTS**: Load `.specify/memory/constitution.md` for project principles and governance constraints.
   - Constitution must be strictly followed.

## Workflow

1. **Analyze changes and identify SDD deviations**:
   - Compare the actual implementation (from git diff) against the original SDD design.
   - Identify any deviations: different data model fields, different API endpoints, different component structure, changed business logic, different permissions model, added/removed features, different tech choices.
   - For each deviation, determine if it:
     - **Replaces** the original design (the SDD should be updated to match the implementation).
     - **Extends** the original design (the SDD should be updated to include the new behaviour).
     - **Simplifies** the original design (the SDD should be updated to reflect the simpler approach with rationale).
   - Compile a list of SDD updates needed.

2. **Ask for developer input**:
   - Present the module name, sprint number, and a summary of detected changes to the developer for confirmation.
   - Ask the developer to:
     - Confirm the sprint title and focus area.
     - Clarify any ambiguous changes detected from git diff.
     - Identify any clarifications or decisions made during implementation that should be documented.
     - Confirm any SDD deviations and whether to update the SDD to match.
     - Identify any new technical debt that should be recorded.
     - Provide a list of new business capabilities added (for business-context-actual.md).
   - Document their responses.

3. **Update as-is docs (Step 13)** — Based on the actual changes and developer input. For each doc, follow the structure defined in its corresponding template in `templates/`, filling in placeholders with actual implementation content:

    #### a. Update `docs/01-as-is/business-context-actual.md`
    - Template: `templates/business_context_template.md`
    - Add new business capabilities as bullet points under Section 1.
    - Update the stakeholder pain points table in Section 3 if any were resolved.
    - Preserve all existing content — only add new entries.

    #### b. Update `docs/01-as-is/system-map.md`
    - Template: `templates/system_map_template.md`
    - Add new API endpoints to the Entry Points table.
    - Update Architecture Diagram, Backend/Frontend Module Maps, and service inventory as needed.

    #### c. Update `docs/01-as-is/data-schema-actual.md`
    - Template: `templates/data_schema_template.md`
    - Add new tables, migrations, and ERD changes with full column definitions.
    - Update the migration count in the opening summary.

    #### d. Update `docs/01-as-is/technical-debt.md`
    - Template: `templates/technical_debt_template.md`
    - Add new items with sequential ID, Description, Area, Priority.
    - Mark any resolved items from this sprint.

4. **Create sprint report (Step 14)** — Generate `docs/03-sprints/sprint-NNN-report.md` using `templates/sprint_report_template.md`:

    - Copy `templates/sprint_report_template.md` to `docs/03-sprints/sprint-NNN-report.md`
    - Replace all `{{PLACEHOLDER}}` variables with actual content from implementation
    - Follow the same structure: Summary, DoD Audit, What Was Delivered, Known Issues, Key Decisions

5. **Create devlog (Step 14)** — Generate `docs/06-devlog/session-YYYY-MM-DD.md` using this format:

   ```markdown
   # Devlog Session — YYYY-MM-DD

   ## [Sprint Title]

   ### Context

   [2-3 sentences describing the context and goals of this session]

   ### Key Changes

   #### Backend — N files

   **Models** (`backend/app/models/`):
   - [file] — [changes]

   **Schemas** (`backend/app/schemas/`):
   - [file] — [changes]

   **API** (`backend/app/api/`):
   - [file] — [changes]

   **Services** (`backend/app/services/`):
   - [file] — [changes]

   **Database** — N migration(s):
   - [migration file] — [changes]

   **Modules** (`modules/`):
   - [file] — [changes]

   #### Frontend — N files

   **Components**:
   - [file] — [changes]

   **Pages**:
   - [file] — [changes]

   **Hooks/Stores**:
   - [file] — [changes]

   **Tests**:
   - [file] — [changes]

   ### SDD Updates

   - [List any SDD sections that were updated to match implementation]

   ### Documentation Updated

   - `docs/01-as-is/business-context-actual.md` — [summary]
   - `docs/01-as-is/system-map.md` — [summary]
   - `docs/01-as-is/data-schema-actual.md` — [summary]
   - `docs/01-as-is/technical-debt.md` — [summary]
   - `docs/02-agile/modules/[module_name]/SDD.md` — [summary]
   - `docs/03-sprints/sprint-NNN-report.md` — Created
   - `docs/06-devlog/session-YYYY-MM-DD.md` — Created
   ```

6. **Update module SDD (additional requirement)** — Update `docs/02-agile/modules/[module_name]/SDD.md` to reflect the actual implementation:

   - **Module Registry table**: Update if version, dependencies, or required role changed.
   - **Data Models**: Update table definitions to match the actual database schema (add/remove/rename columns, change types, update constraints). Update Alembic migration blocks to reference the actual migration revisions. Add new tables that were created during implementation.
   - **API Endpoints**: Update the endpoint table to match actual routes. Add any new endpoints that were implemented. Remove any endpoints from the original SDD that were not implemented.
   - **API Schemas**: Update Pydantic model definitions to match actual request/response schemas.
   - **API Endpoint Implementations**: Update route definitions to match actual implementation.
   - **Frontend Component**: Update the component tree, file structure, and UI patterns to match what was actually built.
   - **Service Layer**: Update business logic descriptions to match the actual implementation.
   - **Module Manifest**: Update if any manifest fields changed.
   - **Integration Points**: Update if integration behaviour differs from the original design.
   - Add a **Sprint N Implementation Notes** subsection at the end of each relevant section documenting what changed and why.
   - Add a new row to the **Change Log**: `| [version] | [date] | [author] | Synced with implementation after Sprint N |`

7. **Commit (Step 15)**:

   **Do NOT commit automatically.** Instead, present the developer with the staging plan:

   ```text
   The following documentation files are ready to be committed:

   Updated as-is docs:
   - docs/01-as-is/business-context-actual.md
   - docs/01-as-is/system-map.md
   - docs/01-as-is/data-schema-actual.md
   - docs/01-as-is/technical-debt.md

   Updated SDD:
   - docs/02-agile/modules/[module_name]/SDD.md

   New sprint doc:
   - docs/03-sprints/sprint-NNN-report.md
   - docs/06-devlog/session-YYYY-MM-DD.md

   Suggested commit message:
   "Update docs: as-is docs, SDD sync, sprint report, devlog"
   ```

   Instruct the developer to review the changes and run:
   ```bash
   git add docs/
   git commit -m "Update docs: as-is docs, SDD sync, sprint report, devlog"
   ```

## Template Reference

All generated docs follow the templates in `templates/`:

| Doc | Template |
|-----|----------|
| Business Context | `templates/business_context_template.md` |
| System Map | `templates/system_map_template.md` |
| Data Schema | `templates/data_schema_template.md` |
| Technical Debt | `templates/technical_debt_template.md` |
| Sprint Report | `templates/sprint_report_template.md` |
| Devlog | (inline format — no template yet) |

## Docs Quality Checklist

Ensure the generated documentation follows these rules:
- [ ] All as-is docs were updated (business-context-actual, system-map, data-schema-actual, technical-debt)
- [ ] New business capabilities are added as bullet points to the correct subsection
- [ ] New API endpoints are added to the Entry Points table under the correct functional area
- [ ] New database tables/migrations are documented in data-schema-actual.md with full column definitions
- [ ] New technical debt items follow the existing numbering scheme
- [ ] Sprint report follows the exact format of sprint-030-report.md
- [ ] Devlog follows the exact format of session-2026-06-17.md
- [ ] Module SDD is fully synced with the actual implementation
- [ ] SDD deviations are documented with rationale in a Sprint Implementation Notes section
- [ ] SDD Change Log is updated with a new entry
- [ ] All PRD requirements remain accurately reflected in the SDD (if a requirement was descoped, note it)
- [ ] As-is docs contain no "TODO" or placeholder content
- [ ] Template placeholders (`{{PLACEHOLDER}}`) are all replaced with actual content
- [ ] Changes are staged and ready for commit (user confirmation obtained before commit)

## References

Always refer to the latest versions of the following:
- **Templates**: `templates/` (all template files)
- **Existing as-is docs**: `docs/01-as-is/` (all files)
- **Existing sprint reports**: `docs/03-sprints/` (for format reference)
- **Existing devlogs**: `docs/06-devlog/` (for format reference)
- **Module PRD**: `docs/02-agile/modules/[module_name]/PRD.md`
- **Module SDD**: `docs/02-agile/modules/[module_name]/SDD.md`
- **Speckit plan**: `specs/[NNN-module-name]/plan.md` (if exists)
- **Speckit tasks**: `specs/[NNN-module-name]/tasks.md` (if exists)
- **Speckit spec**: `specs/[NNN-module-name]/spec.md` (if exists)
