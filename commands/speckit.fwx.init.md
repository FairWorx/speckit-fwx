---
description: Initialize the FairWorx documentation structure — verify commands, create docs folders, seed baseline docs from templates
---

<!-- Extension: fwx -->
<!-- Config: .specify/extensions/fwx/ -->

# Initialize FairWorx Docs Structure

Verifies that all `speckit.fwx.*` commands are available and bootstraps the `docs/` directory with the required folder structure and initial baseline docs from templates.

## Workflow

1. **Check commands** — Verify all required `speckit.fwx.*.md` files exist in `commands/`:

   ```
   Required commands:
   - speckit.fwx.backlog.md        — App backlog
   - speckit.fwx.prd.md             — App PRD
   - speckit.fwx.sdd.md             — App SDD
   - speckit.fwx.requirements.md    — App requirements
   - speckit.fwx.docs.md            — Sprint docs & SDD sync
   - speckit.fwx.init.md            — This command
   - speckit.fwx.mod-backlog.md     — Module backlog
   - speckit.fwx.mod-prd.md         — Module PRD
   - speckit.fwx.mod-sdd.md         — Module SDD
   - speckit.fwx.mod-requirements.md — Module requirements
   ```

   Missing commands are reported. Any missing commands should be flagged but do not block the rest of the init.

2. **Check templates** — Verify all required template files exist in `templates/`:

   ```
   Required templates:
   - app_backlog_template.md
   - app_prd_template.md
   - app_sdd_template.md
   - module_backlog_template.md
   - module_prd_template.md
   - module_sdd_template.md
   - business_context_template.md
   - system_map_template.md
   - data_schema_template.md
   - technical_debt_template.md
   - sprint_report_template.md
   ```

3. **Create docs directory structure** — Ensure the following directories exist (create if missing):

   - `docs/`
   - `docs/01-as-is/`
   - `docs/02-agile/`
   - `docs/03-sprints/`
   - `docs/06-devlog/`

4. **Seed baseline docs** — If a target directory is empty, copy the template file to seed initial baseline docs:

   | Destination | Source Template |
   |-------------|-----------------|
   | `docs/01-as-is/business-context-actual.md` | `templates/business_context_template.md` |
   | `docs/01-as-is/system-map.md` | `templates/system_map_template.md` |
   | `docs/01-as-is/data-schema-actual.md` | `templates/data_schema_template.md` |
   | `docs/01-as-is/technical-debt.md` | `templates/technical_debt_template.md` |
   | `docs/03-sprints/sprint-001-report.md` | `templates/sprint_report_template.md` |

   If a target directory already has files, skip seeding for that directory and report existing files.

5. **Create placeholder READMEs** — Add a `.gitkeep` file to any empty subdirectories under `docs/` to ensure they are tracked by git.

6. **Report results** — Present the developer with a summary:

   ```text
   ┌─────────────────────────────────────────────────┐
   │              speckit.fwx init report             │
   ├─────────────────────────────────────────────────┤
   │ Commands:    10/10 present                      │
   │ Templates:   11/11 present                      │
   │ docs/:       Created                            │
   │   ├── 01-as-is/   - Seeded from templates (4)  │
   │   ├── 02-agile/   - Created                     │
   │   ├── 03-sprints/ - Seeded from template (1)   │
   │   └── 06-devlog/  - Created                     │
   └─────────────────────────────────────────────────┘
   ```

## Post-Init Instructions

After initialization, the developer should:

1. Fill in the `{{PLACEHOLDER}}` variables in each seeded as-is doc with actual project information.
2. Verify the docs directory structure matches the expected layout.
3. Commit the initialized structure.
