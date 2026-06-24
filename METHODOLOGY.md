# FairDev Methodology: AI-Agile Solution Development - v05


## 1. Core Philosophy

This methodology treats the **existing codebase** as the primary source of truth. Documentation is not only important artifacts for **human insight and decision making**, but also a **Machine-Readable Knowledge Base** that allows AI agents to understand not just the code, but the  **stakeholder intentions**, **business objectives**, and **technical constraints** of the legacy system. AI-assisted development must be **specification-driven** with the **human taking responsibility and ownership** of what is being-built and **AI as enabler** for **significant improvement in productivity and quality** never before possible.



## 2. Project Repository Structure

The `docs/` folder is the "Brain" of the repository.

```
root/
├── docs/
│   ├── 01-as-is/                          # CURRENT REALITY
│   │   ├── business-context-actual.md  # Legacy intent + Stakeholder Pain Points
│   │   ├── system-map.md               # Architecture & Entry Points
│   │   ├── data-schema-actual.md       # Real-world DB structure
│   │   └── technical-debt.md           # Friction points vs. Constitution
│   ├── 02-agile/                       # DESIRED FUTURE
│   │   ├── modules
│   │   │   └── mod_module_A   
│   │   │      ├── PRD.md               # Strategy & High-level Features
│   │   │      ├── SDD.md               # Technical Logic & Functional Reqs
│   │   │      └── BACKLOG.md           # Atomic Work Items (PBIs)
│   │   ├── PRD.md                      # Strategy & High-level Features
│   │   ├── SDD.md                      # Technical Logic & Functional Reqs
│   │   └── BACKLOG.md                  # Atomic Work Items (PBIs)
│   ├── 03-sprints/                     # HISTORY & VERIFICATION
│   │   ├── sprint-XX-plan.md           # Current Sprint Goal & PBI Selection
│   │   ├── sprint-XX-report.md         # Evidence of completion & As-Is Sync
│   │   └── PBI/                        # Project backlog item definitions
│   ├── 04-verification/                # TESTING & QUALITY
│   │   ├── test-plan.md                # Scenarios (AI-Auto vs. Human-Manual)
│   │   └── test-reports/               # Execution logs & Bug tracking
│   ├── 05-templates/                   # TEMPLATES
│   │   ├── business-context-actual.md  # Legacy intent + Stakeholder Pain Points
│   │   ├── system-map.md               # Architecture & Entry Points
│   │   ├── data-schema-actual.md       # Real-world DB structure
│   │   ├── technical-debt.md           # Friction points vs. Constitution
│   │   ├── PRD.md                      # Strategy & High-level Features
│   │   ├── SDD.md                      # Technical Logic & Functional Reqs
│   │   ├── BACKLOG.md                  # Atomic Work Items (PBIs)
│   │   ├── sprint-XX-plan.md           # Current Sprint Goal & PBI Selection
│   │   └── sprint-XX-report.md         # Evidence of completion & As-Is Sync
│   ├── 06-DEVLOG/                      # Development logs
│   │   ├── handover.md                 # Notes for handover to next session
│   │   └── session-yyyy-mm-dd.md       # Notes on sessions
│   ├── 07-DEVGUIDE/                    # Guidelines for Developers
│   ├── 08-USERGUIDE/                   # Manuals and Guidelines for Users
│   ├── METHODOLOGY.md                  # This Framework Document
│   └── CONSTITUTION.md                 # Coding Standards & AI Guardrails
├── src/                                # IMPLEMENTATION
└── .spec-kit/                          # AI AGENT CONFIG
```

------



## 3. The 3-Phase Lifecycle

The workflow follows three phases as defined in `WORKFLOW.md`. Each phase produces artifacts consumed by the next.

### Phase 1: Application Definition

Define *what* the application does at a product and design level using `speckit.fwx` commands:

1. **Requirements (optional):** `/speckit.fwx.requirements` — structure raw input into `docs/02-agile/requirements.md`, or write by hand.
2. **PRD (required):** `/speckit.fwx.prd` — produce `docs/02-agile/PRD.md` defining features, data, API, UI, and architecture.
3. **SDD (optional):** `/speckit.fwx.sdd` — produce `docs/02-agile/SDD.md` for complex applications; rely on per-sprint planning for simple ones.
4. **Backlog (required):** `/speckit.fwx.backlog` — break PRD into Epics and PBIs in `docs/02-agile/BACKLOG.md`.

### Phase 2: Sprint Execution

Each sprint selects PBIs from the backlog and executes them through the core Speckit pipeline:

1. **Specify:** `/speckit.specify PBI/Epic from BACKLOG.md` — create `specs/[NNN-feature]/spec.md`.
2. **Plan:** `/speckit.plan` — generate `plan.md` with data model, contracts, and implementation steps.
3. **Tasks:** `/speckit.tasks` — break the plan into ordered tasks in `tasks.md`.
4. **Implement:** `/speckit.implement` — execute all tasks, writing code and tests.
5. **Converge:** `/speckit.converge` — verify completeness against the spec. Loop back to implement if gaps found.

### Phase 3: Documentation & Closure

After converge reports "✅ Converged", finalise the sprint:

1. **Docs:** `/speckit.fwx.docs` — generate sprint report, devlog, update as-is docs, sync SDD.
2. **Commit:** Review staged changes, then `git add && git commit`.

**As-Is Update:** AI updates `docs/01-as-is/` (business context, system map, data schema, technical debt) to reflect the new code baseline. Resolved pain points are marked; new technical debt is recorded.

**Evaluation:** Compare the updated as-is state against `PRD.md`. Decide whether to continue with the next sprint (Phase 2) or revise the design (Phase 1).

------



## 4. Templates

All baseline documents have templates in `templates/`. Use `/speckit.fwx.init` to seed the docs folder from templates.

| Document | Template |
|----------|----------|
| Business Context | `templates/business_context_template.md` |
| System Map | `templates/system_map_template.md` |
| Data Schema | `templates/data_schema_template.md` |
| Technical Debt | `templates/technical_debt_template.md` |
| Sprint Report | `templates/sprint_report_template.md` |
| App PRD | `templates/app_prd_template.md` |
| App SDD | `templates/app_sdd_template.md` |
| App Backlog | `templates/app_backlog_template.md` |
| Module PRD | `templates/module_prd_template.md` |
| Module SDD | `templates/module_sdd_template.md` |
| Module Backlog | `templates/module_backlog_template.md` |



------



## 5. Integrating Speckit

Speckit is the execution engine. It requires accurate documentation in `docs/` and `specs/` to avoid hallucinations.

- **Constitution:** Run `/speckit.constitution` at project setup (or when amending rules) to generate `.specify/memory/constitution.md`. Every subsequent command reads the constitution to ensure compliance.
- **Instructional Context:** Before any speckit command, read `@system-map.md` and `@data-schema-actual.md` for full context.
- **Safety:** The `/speckit.plan` command must explicitly check for existing functions in `system-map.md` to prevent duplication.
- **Context Injection:** When issuing `/speckit.implement`, append the relevant architecture files, e.g., `/speckit.implement [PBI-ID] using @system-map.md and @data-schema-actual.md`.
- **Verification Loop:** Every `/speckit.implement` must execute the test suite and self-correct until green.
- **Converge Gate:** Always run `/speckit.converge` after implement. Loop back to implement if gaps are found. Only proceed to `/speckit.fwx.docs` after converge reports "✅ Converged".

------



## 6. AI Command Manual (Prompts)

### Phase 1: Application Definition

> "Run `/speckit.fwx.requirements [description]` to structure raw input into `docs/02-agile/requirements.md`, or write it by hand."
>
> "Run `/speckit.fwx.prd` to produce `docs/02-agile/PRD.md` from the requirements."
>
> "Run `/speckit.fwx.sdd` to produce `docs/02-agile/SDD.md` (optional — skip for simple apps)."
>
> "Run `/speckit.fwx.backlog` to break the PRD into Epics and PBIs in `docs/02-agile/BACKLOG.md`."

### Phase 2: Sprint Execution

> "Run `/speckit.specify PBI-{Epic#}-{seq} from docs/02-agile/BACKLOG.md` to create a feature specification."
>
> "Run `/speckit.clarify` to resolve ambiguities (optional)."
>
> "Run `/speckit.plan` referencing @system-map.md and @data-schema-actual.md to ensure reuse."
>
> "Run `/speckit.tasks` to break the plan into ordered tasks."
>
> "Run `/speckit.implement` to build it. Test manually for bugs."
>
> "Run `/speckit.converge` to find remaining gaps. Loop implement → converge until '✅ Converged'."

### Phase 3: Documentation & Closure

> "The implementation for **Sprint [XX]** is complete and converged. Run `/speckit.fwx.docs` to generate sprint report, devlog, and update as-is docs. The command will:
> 1. Analyze deviations between implementation and documented design.
> 2. Update `docs/01-as-is/` (business context, system map, data schema, technical debt).
> 3. Create sprint report at `docs/03-sprints/sprint-NNN-report.md`.
> 4. Create devlog at `docs/06-devlog/session-YYYY-MM-DD.md`.
> 5. Stage changes for review."
>
> "After `/speckit.fwx.docs` completes, review the staged changes and commit: `git add docs/ && git commit`."
>
> "Compare the updated `docs/01-as-is/` state against `docs/02-agile/PRD.md`. Will the architecture achieve the business KPIs? If not, propose design revisions before the next sprint."

------



## 7. Master System Instructions

*Apply these to your AI agent's "System Instructions" or "Custom Profile".*

**Role:** AI-Agile Development Partner. 

**Mandate:** 

1. **Constitution Adherence:** Strictly follow all testing and coding standards defined in `.specify/memory/constitution.md`. 
2. **Implicit Verification:** Testing is not an optional step; it is part of the `/speckit.implement` action. You are not "done" until the code passes the tests you wrote for it. 
3. **Traceability:** Maintain the link: `Code → PBI → FR/NFR → Feature`. 
4. **Context First:** Always read `docs/01-as-is/` files to prevent regressions in legacy logic.
5. **Workflow Compliance:** Follow the 3-phase workflow in `WORKFLOW.md`. Do not skip the converge step before running docs.





------

## 8. AI System Instructions: AI-Agile Framework

To finalize your framework, here is the **AI System Instructions (Master Prompt)**.

You can paste this into the "System Instructions" or "Custom Instructions" of your AI agent (ChatGPT, Claude, or a specialized IDE agent like SPEC-KIT). It ensures the AI understands its role as a partner in this specific Agile lifecycle.



**Role:** You are an AI-Agile Development Partner. Your goal is to assist in a Spec-Driven development workflow following the 3-phase lifecycle defined in `WORKFLOW.md`.

**Core Directive:** Never generate code or plans in isolation. Always cross-reference the `docs/` folder to ensure alignment with Business Context, Technical Design, and the current As-Is state.

### 1. Document Awareness

You must always interpret the repository using the following hierarchy:

- **`docs/01-as-is/`**: The Current Reality. Use this to avoid regressions and understand legacy constraints.
- **`docs/02-agile/`**: The Desired Future. Use this for requirements (`PRD.md`, `SDD.md`, `BACKLOG.md`).
- **`docs/03-sprints/`**: The active work order and sprint history.
- **`templates/`**: Baseline templates for generating documentation.

### 2. Operating Principles

1. **Respect Legacy Intent:** Before refactoring, check `docs/01-as-is/business-context-actual.md` for "Sacred Logic."
2. **Traceability:** Every code change must be traceable: `Code → PBI → FR/NFR → Feature`.
3. **DRY (Don't Repeat Yourself):** Always check `docs/01-as-is/system-map.md` before suggesting new utilities or components.
4. **Binary Completion:** Acceptance Criteria (AC) in PBIs are binary. They are either 100% met or the task is incomplete.
5. **Test-Driven Development:** Always generate tests and run them as part of the implementation process.
6. **Data Model Changes:** When there is a need to change data model or data tables, always check with the human first.
7. **Converge Before Docs:** Never proceed to `/speckit.fwx.docs` until `/speckit.converge` reports "✅ Converged".

### 3. Workflow Phase Instructions

#### Phase 1: Application Definition

Use `speckit.fwx` commands to produce the application definition artifacts. Each command reads the previous artifact as input.

#### Phase 2: Sprint Execution

Follow the core Speckit pipeline for each PBI or Epic:

- **Specify:** Create `specs/[NNN-feature]/spec.md` with exhaustive logic and edge cases.
- **Plan:** Generate `plan.md` referencing `system-map.md` and `data-schema-actual.md` to avoid duplication.
- **Tasks:** Break the plan into ordered implementation tasks.
- **Implement:** Write code and tests, then run the test suite. Self-correct until green.
- **Converge:** Verify completeness against the spec. Loop back to implement if gaps exist.

#### Phase 3: Documentation & Closure

Run `/speckit.fwx.docs` to finalise the sprint. Review staged changes, then commit.



---



## 9. Definition of Done (DoD)

A Product Backlog Item (PBI) or Sprint is only considered **Done** when it meets the following criteria across three levels of verification:

### 9.1. Technical Completion (The AI Gate)

- **Code Standard:** The implementation follows all coding and architectural standards defined in the constitution (`.specify/memory/constitution.md`).
- **No Duplication:** Verified against `docs/01-as-is/system-map.md` that no redundant functions or logic were created.
- **Automated Pass:** 100% of the automated tests written during implementation are passing.
- **Traceability:** Code is linked to the specific PBI-ID and Functional Requirement (FR) ID.
- **Converged:** `/speckit.converge` reports "✅ Converged" with no critical or high gaps.

### 9.2. Quality Assurance (The Human Gate)

- **Acceptance Criteria (AC):** All binary ACs defined in `docs/02-agile/BACKLOG.md` for the specific PBI are met.
- **Manual Scenarios:** Manual test cases executed and passed by a human.
- **Review:** The human has reviewed the implementation plan and final code for alignment with stakeholder intent.

### 9.3. Documentation & Baseline Sync (The Repository Gate)

- **As-Is Update:** `docs/01-as-is/` (business context, system map, data schema, technical debt) updated to reflect the new codebase state.
- **Sprint Report:** `docs/03-sprints/sprint-NNN-report.md` created as a permanent record.
- **Devlog:** `docs/06-devlog/session-YYYY-MM-DD.md` created as a development log.





------

#### The Manifest: AI-Agile Development Framework

*Copy the content below into `docs/METHODOLOGY.md` for your project's permanent record.*

Markdown

```
# AI-Agile Development Framework (Spec-Driven)

## 1. Philosophy
This project uses a documentation-first, AI-assisted workflow. Documentation is the "Source of Truth" for both humans and AI agents.

## 2. Lifecycle & Phases

### Phase 1: Application Definition
- **Goal:** Define what the application does.
- **Commands:** `speckit.fwx.requirements`, `speckit.fwx.prd`, `speckit.fwx.sdd`, `speckit.fwx.backlog`
- **Artifacts:** `docs/02-agile/requirements.md`, `PRD.md`, `SDD.md`, `BACKLOG.md`

### Phase 2: Sprint Execution
- **Goal:** Implement PBIs via the Speckit pipeline.
- **Commands:** `specify`, `plan`, `tasks`, `implement`, `converge`
- **Artifacts:** `specs/[NNN-feature]/` (spec, plan, tasks, contracts, data-model)

### Phase 3: Documentation & Closure
- **Goal:** Finalise the sprint and sync documentation.
- **Commands:** `speckit.fwx.docs`
- **Artifacts:** `docs/03-sprints/sprint-NNN-report.md`, `docs/06-devlog/session-YYYY-MM-DD.md`

## 3. Traceability Map

1. Code → PBI (Backlog)
2. PBI → FR/NFR (SDD)
3. FR/NFR → Feature (PRD)
4. Feature → Pain Point (Business Context)
```





