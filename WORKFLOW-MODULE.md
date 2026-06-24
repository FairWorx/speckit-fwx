# FairWorx Module Development Workflow

**Purpose:** This document describes the complete workflow for building FairWorx platform modules using the speckit.fwx extension combined with the core Speckit commands.

---

## Overview

The workflow is divided into three phases:

```
Phase 1: Module Definition (fwx extension)
  [requirements] → prd → [sdd] → backlog

Phase 2: Sprint Execution (core speckit)
  specify → plan → tasks → implement → converge ──→ [implement → converge] → docs
                                                      ↑  loop until converged  │
                                                                               │
Phase 3: Documentation & Closure (fwx extension + speckit)                    │
  [analyze] → [checklist] → docs ←─────────────────────────────────────────────┘
                             → [taskstoissues]
```

Each phase produces artifacts in `docs/02-agile/modules/[module_name]/` that feed into the next phase.

For the official documentation of Github's Speckit workflow, please visit https://github.com/github/spec-kit#-detailed-process

---

## Project Foundation (One-Time Setup)

### Constitution

```
/speckit.constitution
```

**Purpose:** Generate or update the project constitution — a set of governing rules that the AI agent must follow. The constitution codifies architectural invariants, tech stack choices, naming conventions, and workflow rules.

**When to run:** The constitution should already exist in any speckit-enabled project at `.specify/memory/constitution.md`. If it does not exist, run `/speckit.constitution` **first**, before any other speckit or speckit.fwx command. Every subsequent command reads the constitution to ensure compliance.

**Importance:**
- The constitution is the **source of truth for architectural decisions**. Every speckit command reads it to ensure generated code and docs comply with project standards.
- It prevents the AI from making choices that contradict the project's established patterns (e.g., using a different UI framework, violating the module naming convention, or writing sync code in an async-first backend).
- It encodes "sacred" logic that must never be broken (e.g., JWT auth, bcrypt cost, audit trail immutability).
- Run it only once when setting up a new project, or when the constitution needs amendment (see Article XI).

**Location:** `.specify/memory/constitution.md`

---

## Phase 1: Module Definition

These commands use the **speckit.fwx** extension. They define *what* the module does at a product and design level, and produce the structured documentation needed before implementation begins.

### 1. Requirements (Optional)

```
/speckit.fwx.mod-requirements <raw description>
```

**Purpose:** Take raw, unstructured user input about a desired feature and produce a structured `requirements.md` file.

**Input:** A free-form description of what the module should do (features, data, UI, integration points).

**Alternative:** Instead of using this command, you can write `requirements.md` by hand in any text editor. The file follows a simple structure (module name, description, core features, channel context, data entities, UI patterns, integration points). Either approach produces the same output consumed by the PRD command.

**Output:** `docs/02-agile/modules/[module_name]/requirements.md`

**When to run:** At the very start, when you have a rough idea of what the module should do but haven't formalised it yet. This is an optional intake step — you can skip directly to PRD if the requirements are already clear.

---

### 2. PRD (Product Requirements Document)

```
/speckit.fwx.mod-prd [module_name]
```

**Purpose:** Produce a formal Product Requirements Document (`PRD.md`) for the module. This defines *what* the module does, its features, data entities, API surface, UI patterns, and integration points with the main app.

**Prerequisites:** Either a requirements file (from the previous step or manually written) or a clear description in the command arguments.

**Input Sources (in priority order):**
1. Existing `docs/02-agile/modules/[module_name]/requirements.md`
2. A file path passed as an argument with a `.txt` or `.md` extension (e.g., `/speckit.fwx.mod-prd path/to/notes.txt`)
3. Raw text in the command arguments

**Validation:** The command cross-references the requirements against the main platform PRD, SDD, and UI docs. Any deviations are surfaced to you with options to proceed, comply, or update the parent docs.

**Output:** `docs/02-agile/modules/[module_name]/PRD.md`

**When to run:** After requirements are gathered. This is the primary definition document for the module. Always create a PRD — it anchors your backlog and provides the "why" behind each feature.

---

### 3. SDD (Solution Design Document) — Optional

```
/speckit.fwx.mod-sdd [module_name]
```

**Purpose:** Produce a detailed Solution Design Document (`SDD.md`) with data models, API schemas, endpoint implementations, frontend component trees, service layer logic, and module manifests.

**Why it's optional:**
- The PRD already captures the data model and API surface at a high level.
- The core speckit commands (`specify → plan → tasks`) generate detailed technical design *per feature* during each sprint, which is more accurate than designing the entire module upfront.
- The `speckit.fwx.docs` command (see Phase 3) syncs any implementation changes back to the SDD, keeping it accurate without requiring perfect upfront design.

**When to run it:**
- **Complex modules** with many entities, intricate business logic, or cross-module dependencies — an upfront SDD helps identify design issues early.
- **Simple modules** can skip this step and rely on `speckit.plan` for per-feature design.
- If you skip it, you can still generate it later via the docs command's SDD sync.

**Output:** `docs/02-agile/modules/[module_name]/SDD.md`

---

### 4. Backlog

```
/speckit.fwx.mod-backlog [module_name]
```

**Purpose:** Break the PRD down into Epics and Product Backlog Items (PBIs) in a `BACKLOG.md` file. This is the bridge between product definition and sprint execution.

**Prerequisites:** A `PRD.md` must exist for the module.

**What it produces:**
- **Epics** — logical groupings of related features (e.g., "Foundation", "Core CRUD", "Reporting").
- **PBIs** — individual user stories with:
  - Hierarchical numbering: `PBI-{Epic#}-{seq}` (e.g., `PBI-12-001`)
  - Priority (P0–P3)
  - Acceptance criteria annotated with test type: `[unit]`, `[integration]`, `[e2e]`, `[human]`
  - Technical guidance and constraints
- **Requirements Traceability Matrix** — a table mapping every PRD requirement to its implementing PBI(s), ensuring full coverage.

**Ordering Rules:**
- Epics are ordered so foundational work (data models, infrastructure) comes first.
- Within each Epic: database migrations → API endpoints → frontend components.
- Read operations before write operations where sensible.
- Simple CRUD before complex business logic.

**Output:** `docs/02-agile/modules/[module_name]/BACKLOG.md`

**When to run:** After the PRD is finalised. The backlog is what you use to plan sprints — pick an Epic or a set of PBIs for each sprint.

---

## Phase 2: Sprint Execution

These are the **core Speckit commands**. Each sprint picks one or more PBIs (or an entire Epic) from the backlog and executes them through this pipeline.

### Constitution

The constitution must already exist (see [Phase 0](#phase-0-project-foundation-one-time-setup) above). If it does not, run `/speckit.constitution` first before proceeding.

---

### Specify

```
/speckit.specify PBI-{Epic#}-{seq} from docs/02-agile/modules/[module_name]/BACKLOG.md
# or
/speckit.specify Epic {Epic#} from docs/02-agile/modules/[module_name]/BACKLOG.md
```

**Purpose:** Create a feature specification (`spec.md`) for a specific PBI or Epic from the backlog. This zooms into one slice of the module and defines exactly what needs to be built in this sprint.

**Key rule:** Always reference the PBI or Epic from the backlog. Do not re-define requirements — the backlog already captures them. The spec is about scoping and clarifying *this sprint's* slice.

**What it produces:**
- `specs/[NNN-module-name]/spec.md` — the feature specification
- A new git branch (if the git extension is enabled)

**Branch naming:** Sequential (`003-module-name`) or timestamp-based.

**Output directory:** `specs/[NNN-module-name]/`

---

### Plan

```
/speckit.plan
```

**Purpose:** Generate an implementation plan (`plan.md`) for the specified feature. This covers:

- **Technical Context** — language, dependencies, storage, testing approach
- **Constitution Check** — validates the plan against all constitutional articles
- **Project Structure** — which files will be created or modified
- **Research** — analysis of existing patterns and potential issues (`research.md`)
- **Data Model** — detailed schema design (`data-model.md`)
- **Contracts** — API contracts and interfaces (`contracts/`)
- **Quickstart** — step-by-step implementation guide (`quickstart.md`)

The plan is the equivalent of a feature-level SDD. It replaces the need for a detailed upfront module SDD by providing just-in-time design for exactly what you're building now.

**Prerequisites:** A `spec.md` must exist (from the specify step).

---

### Tasks

```
/speckit.tasks
```

**Purpose:** Break the plan into individual implementation tasks (`tasks.md`) organized by phase:

- **Phase 1: Setup** — environment, configuration, scaffolding
- **Phase 2: Foundational** — data models, migrations, base classes
- **Phase 3: User Stories** — implementation ordered by P1 → P2 → P3 priority
- **Phase N: Polish** — edge cases, error handling, cleanup

Each task includes a task ID, priority, user story reference, and description. Tasks are ordered so that dependencies are respected (migrations before API, API before frontend).

**Prerequisites:** A `plan.md` must exist.

---

### Implement

```
/speckit.implement
```

**Purpose:** Execute all tasks from `tasks.md` in order. The AI reads each task and implements it — writing backend code (models, schemas, API endpoints, services, migrations) and frontend code (components, hooks, stores) as specified.

**What to watch for:**
- Monitor the output for errors or unexpected behaviour.
- After implementation, test manually (see the developer workflow guide for commands).
- If bugs are found, fix them before moving to Phase 3.

---

### Converge

```
/speckit.converge
```

**Purpose:** Close the gap between what the specification, plan, and tasks call for and what the codebase currently implements. This is the **implementation completeness gate** — it finds remaining work so nothing is forgotten.

**Prerequisites:** `spec.md`, `plan.md`, and `tasks.md` must all exist. Run this only after `/speckit.implement` has completed.

**What it does:**

1. **Loads the intent inventory** — reads `spec.md` (functional requirements, success criteria, user story acceptance scenarios), `plan.md` (architecture decisions, data models, file touch-points), and `tasks.md` (existing task list).
2. **Assesses the codebase** — inspects current code against every intent item. Classifies each gap as:
   - `missing` — required work is absent
   - `partial` — work exists but doesn't fully satisfy the requirement
   - `contradicts` — code conflicts with stated intent or constitution MUST rules
   - `unrequested` — code exists without being called for by spec/plan/tasks
3. **Reports findings** — outputs a severity-graded summary (CRITICAL/HIGH/MEDIUM/LOW).
4. **Appends convergence tasks** — if gaps exist, appends a new `## Phase N: Convergence` section to `tasks.md` with new tasks. If no gaps exist, reports **"✅ Converged"** and leaves `tasks.md` unchanged.

**Append-only rule:** Converge never modifies existing tasks or artifacts. It only appends new work to `tasks.md`.

**Converge loop:**

```
/speckit.implement → /speckit.converge
                           │
                    ┌──────┴──────┐
                    ▼              ▼
              gaps found       converged
                    │              │
                    ▼              ▼
           /speckit.implement    proceed to
                │               Phase 3: docs
                ▼
          /speckit.converge (repeat)
```

After converge appends tasks, run `/speckit.implement` again to complete them, then `/speckit.converge` again. Repeat until converge reports "✅ Converged". Only then move to documentation.

**Important:** Converge and `speckit.fwx.docs` serve different purposes:
- **Converge** ensures the *code* is complete against the spec.
- **Docs** ensures the *documentation* reflects the completed code.

Run converge (possibly multiple rounds) first, then docs once.

---

## Phase 3: Documentation & Closure

### Docs

```
/speckit.fwx.docs [module_name]
```

**Purpose:** Generate sprint documentation and synchronise the SDD with the actual implementation. This runs **after** converge reports "✅ Converged" — it ensures that documentation accurately reflects what was built, not what was planned.

**What it does:**

1. **Analyzes deviations** — compares the actual implementation (via `git diff`) against the original SDD design. Identifies every difference: added/removed/renamed fields, changed API routes, different component structure, modified business logic.

2. **Updates as-is docs** (`docs/01-as-is/`):
   - `business-context-actual.md` — adds new capabilities to the feature inventory
   - `system-map.md` — adds new API endpoints, updates architecture map and module directory trees
   - `data-schema-actual.md` — documents new tables, migrations, and schema changes
   - `technical-debt.md` — records any new technical debt incurred during the sprint

3. **Creates sprint report** (`docs/03-sprints/sprint-NNN-report.md`):
   - Sprint summary and Definition of Done audit (Technical, QA, Documentation gates)
   - What was delivered (backend changes, frontend changes)
   - Clarifications resolved during implementation
   - Remaining work and known issues

4. **Creates devlog** (`docs/06-devlog/session-YYYY-MM-DD.md`):
   - Session context and goals
   - Key changes organized by layer (models, schemas, API, services, database, modules, frontend components, hooks)
   - Tests added
   - Documentation updated

5. **Syncs the module SDD** — updates every section of the SDD to match the actual implementation:
   - Data models: field definitions, types, constraints, migration revisions
   - API endpoints: actual routes, methods, descriptions
   - API schemas: actual request/response models
   - Frontend components: actual file structure and component tree
   - Service layer: actual business logic
   - Integration points: actual integration behaviour
   - Adds "Sprint N Implementation Notes" subsections documenting what changed and why
   - Updates the Change Log

6. **Stages for commit** — presents the file manifest and suggested commit message. Review and commit manually.

**When to run:** At the end of every sprint, after implementation and bug fixes are complete.

---

## Optional Speckit Commands

These commands are available but not part of the core workflow. Use them when you need additional analysis, quality assurance, or project management integration.

### Clarify

```
/speckit.clarify
```

**Purpose:** Resolve ambiguous or missing requirements in the specification. After running `/speckit.specify`, the spec may contain open questions or underspecified areas. This command presents those questions to you and captures your answers.

**When to use:**
- After `/speckit.specify` if the generated spec has placeholder values or noted ambiguities.
- When you realise a requirement is not well-defined enough to implement.
- The command updates the spec with your clarifications and can trigger a re-plan if needed.

---

### Analyze

```
/speckit.analyze
```

**Purpose:** Review the implemented code against the specification and identify issues, gaps, or improvements. This is a code review / gap analysis tool.

**What it checks:**
- Whether all requirements from the spec are implemented
- Whether the implementation follows constitutional rules
- Whether there are any obvious bugs, security issues, or performance problems
- Whether error handling and edge cases are covered

**When to use:**
- After `/speckit.implement` completes, before manual testing.
- When you want a second-pass review of the implementation quality.
- Can be run at any point during implementation to check progress.

---

### Checklist

```
/speckit.checklist
```

**Purpose:** Generate a quality checklist (`checklist.md`) for the current feature. This is a structured list of verification items organised by category (e.g., Backend, Frontend, Security, Testing, Documentation).

**When to use:**
- Before creating a pull request, to ensure all quality criteria are met.
- During code review, as a structured reference for what to check.
- The checklist is customised to the specific feature being built.

---

### Taskstoissues

```
/speckit.taskstoissues
```

**Purpose:** Sync implementation tasks from `tasks.md` to GitHub Issues. Each task becomes a GitHub issue with appropriate labels, assignees, and linked relationships (blocked by / blocks).

**When to use:**
- When you want to track implementation progress on GitHub instead of locally.
- For larger projects where multiple developers need visibility into task status.
- Requires GitHub authentication (`gh` CLI) and appropriate repository permissions.

---

## Quick Reference

```
Phase 0: Project Foundation (one-time)
─────────────────────────────────────────────
/speckit.constitution                   (if missing — project governing rules)

Phase 1: Module Definition
─────────────────────────────────────────────
/speckit.fwx.mod-requirements <desc>    (optional — structured intake, or write by hand)
/speckit.fwx.mod-prd [path|name]        (required — product requirements)
/speckit.fwx.mod-sdd [name]             (optional — solution design)
/speckit.fwx.mod-backlog [name]         (required — epics & PBIs)

Phase 2: Sprint Execution (per sprint)
─────────────────────────────────────────────
/speckit.specify PBI/Epic from BACKLOG  (required — scope this sprint)
/speckit.clarify                        (optional — resolve ambiguities)
/speckit.plan                           (required — implementation plan)
/speckit.tasks                          (required — task breakdown)
/speckit.implement                      (required — build it)
  └─ manual testing & bug fixes
/speckit.converge                       (required — find remaining gaps)
  └─ loop: implement → converge until "✅ Converged"
/speckit.analyze                        (optional — implementation review)
/speckit.checklist                      (optional — quality checklist)

Phase 3: Documentation & Closure
─────────────────────────────────────────────
/speckit.fwx.docs [name]            (required — sprint docs + SDD sync)
/speckit.taskstoissues                  (optional — sync tasks to GitHub Issues)
  └─ git add + git commit               (manual — after reviewing changes)

Related Documents
─────────────────────────────────────────────
- Constitution: .specify/memory/constitution.md
- Developer workflow: docs/04-guides/developer-setup-and-workflow.md
- Platform PRD: docs/02-agile/PRD.md
- Platform SDD: docs/02-agile/SDD.md
