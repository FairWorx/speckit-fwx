# FairWorx Application Development Workflow

**Purpose:** This document describes the complete workflow for building a greenfield FairWorx application using the speckit.fwx extension combined with the core Speckit commands.

---

## Overview

The workflow is divided into three phases:

```
Phase 1: Application Definition (fwx extension)
  [requirements] → prd → [sdd] → backlog

Phase 2: Sprint Execution (core speckit)
  specify → plan → tasks → implement → converge ──→ [implement → converge] → docs
                                                      ↑  loop until converged  │
                                                                                │
Phase 3: Documentation & Closure (fwx extension + speckit)                    │
  [analyze] → [checklist] → docs ←─────────────────────────────────────────────┘
                             → [taskstoissues]
```

Each phase produces artifacts in `docs/02-agile/` that feed into the next phase.

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
- It prevents the AI from making choices that contradict the project's established patterns.
- It encodes "sacred" logic that must never be broken.
- Run it only once when setting up a new project, or when the constitution needs amendment.

**Location:** `.specify/memory/constitution.md`

---

## Phase 1: Application Definition

These commands use the **speckit.fwx** extension. They define *what* the application does at a product and design level, and produce the structured documentation needed before implementation begins.

### 1. Requirements (Optional)

```
/speckit.fwx.requirements <raw description>
```

**Purpose:** Take raw, unstructured user input about a desired application and produce a structured `requirements.md` file.

**Input:** A free-form description of what the application should do (features, data, UI, personas).

**Alternative:** Instead of using this command, you can write `requirements.md` by hand in any text editor. Either approach produces the same output consumed by the PRD command.

**Output:** `docs/02-agile/requirements.md`

**When to run:** At the very start, when you have a rough idea of what the application should do but haven't formalised it yet. This is an optional intake step — you can skip directly to PRD if the requirements are already clear.

---

### 2. PRD (Product Requirements Document)

```
/speckit.fwx.prd
```

**Purpose:** Produce a formal Product Requirements Document (`PRD.md`) for the application. This defines *what* the application does, its features, data entities, API surface, UI/UX direction, technical architecture, and security requirements.

**Prerequisites:** Either a requirements file (from the previous step or manually written) or a clear description in the command arguments.

**Input Sources (in priority order):**
1. Existing `docs/02-agile/requirements.md`
2. Raw text in the command arguments
3. Prompted input if neither exists

**Output:** `docs/02-agile/PRD.md`

**When to run:** After requirements are gathered. This is the primary definition document for the application. Always create a PRD — it anchors your backlog and provides the "why" behind each feature.

---

### 3. SDD (Solution Design Document) — Optional

```
/speckit.fwx.sdd
```

**Purpose:** Produce a detailed Solution Design Document (`SDD.md`) with data models, API schemas, frontend architecture, backend architecture, and deployment details.

**Why it's optional:**
- The PRD already captures the data model and API surface at a high level.
- The core speckit commands (`specify → plan → tasks`) generate detailed technical design *per feature* during each sprint, which is more accurate than designing the entire application upfront.
- Phase 3 docs command syncs any implementation changes back to the SDD, keeping it accurate without requiring perfect upfront design.

**When to run it:**
- **Complex applications** with many entities, intricate business logic, or multiple services — an upfront SDD helps identify design issues early.
- **Simple applications** can skip this step and rely on `/speckit.plan` for per-feature design.

**Output:** `docs/02-agile/SDD.md`

---

### 4. Backlog

```
/speckit.fwx.backlog
```

**Purpose:** Break the PRD down into Epics and Product Backlog Items (PBIs) in a `BACKLOG.md` file. This is the bridge between product definition and sprint execution.

**Prerequisites:** A `PRD.md` must exist for the application.

**What it produces:**
- **Epics** — logical groupings of related features (e.g., "Foundation", "Core CRUD", "Reporting").
- **PBIs** — individual user stories with:
  - Hierarchical numbering: `PBI-{Epic#}-{seq}` (e.g., `PBI-12-001`)
  - Priority (P0–P3)
  - Acceptance criteria annotated with test type: `[unit]`, `[integration]`, `[e2e]`, `[human]`
  - Technical guidance and constraints
- **Requirements Traceability Matrix** — a table mapping every PRD requirement to its implementing PBI(s), ensuring full coverage.

**Ordering Rules:**
- Epics are ordered so foundational work (auth, data models, infrastructure) comes first.
- Within each Epic: database migrations → API endpoints → frontend components.
- Read operations before write operations where sensible.
- Simple CRUD before complex business logic.

**Output:** `docs/02-agile/BACKLOG.md`

**When to run:** After the PRD is finalised. The backlog is what you use to plan sprints — pick an Epic or a set of PBIs for each sprint.

---

## Phase 2: Sprint Execution

These are the **core Speckit commands**. Each sprint picks one or more PBIs (or an entire Epic) from the backlog and executes them through this pipeline.

### Constitution

The constitution must already exist (see Project Foundation above). If it does not, run `/speckit.constitution` first before proceeding.

---

### Specify

```
/speckit.specify PBI-{Epic#}-{seq} from docs/02-agile/BACKLOG.md
# or
/speckit.specify Epic {Epic#} from docs/02-agile/BACKLOG.md
```

**Purpose:** Create a feature specification (`spec.md`) for a specific PBI or Epic from the backlog. This zooms into one slice of the application and defines exactly what needs to be built in this sprint.

**Key rule:** Always reference the PBI or Epic from the backlog. Do not re-define requirements — the backlog already captures them.

**Output directory:** `specs/[NNN-feature-name]/`

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
- After implementation, test manually.
- If bugs are found, fix them before moving to Phase 3.

---

### Converge

```
/speckit.converge
```

**Purpose:** Close the gap between what the specification, plan, and tasks call for and what the codebase currently implements. This is the **implementation completeness gate**.

**Prerequisites:** `spec.md`, `plan.md`, and `tasks.md` must all exist. Run this only after `/speckit.implement` has completed.

**What it does:**
1. Loads the intent inventory from spec, plan, and tasks.
2. Assesses the codebase against every intent item.
3. Reports findings with severity grading (CRITICAL/HIGH/MEDIUM/LOW).
4. Appends convergence tasks if gaps exist.

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

**Important:** Converge and `/speckit.fwx.docs` serve different purposes:
- **Converge** ensures the *code* is complete against the spec.
- **Docs** ensures the *documentation* reflects the completed code.

Run converge (possibly multiple rounds) first, then docs once.

---

## Phase 3: Documentation & Closure

### Docs

```
/speckit.fwx.docs
```

**Purpose:** Generate sprint documentation and synchronise with documentation as needed. This runs **after** converge reports "✅ Converged".

**What it does:**
1. Analyzes deviations between implementation and documented design.
2. Updates as-is docs (`docs/01-as-is/`).
3. Creates sprint report (`docs/03-sprints/sprint-NNN-report.md`).
4. Creates devlog (`docs/06-devlog/session-YYYY-MM-DD.md`).
5. Stages for commit — presents the file manifest and suggested commit message.

**When to run:** At the end of every sprint, after implementation and bug fixes are complete.

---

## Optional Speckit Commands

These commands are available but not part of the core workflow.

### Clarify

```
/speckit.clarify
```

**Purpose:** Resolve ambiguous or missing requirements in the specification.

---

### Analyze

```
/speckit.analyze
```

**Purpose:** Review the implemented code against the specification and identify issues, gaps, or improvements.

---

### Checklist

```
/speckit.checklist
```

**Purpose:** Generate a quality checklist (`checklist.md`) for the current feature.

---

### Taskstoissues

```
/speckit.taskstoissues
```

**Purpose:** Sync implementation tasks from `tasks.md` to GitHub Issues.

---

## Quick Reference

```
Phase 0: Project Foundation (one-time)
─────────────────────────────────────────────
/speckit.constitution                   (if missing — project governing rules)

Phase 1: Application Definition
─────────────────────────────────────────────
/speckit.fwx.requirements <desc>    (optional — structured intake, or write by hand)
/speckit.fwx.prd                    (required — product requirements)
/speckit.fwx.sdd                    (optional — solution design)
/speckit.fwx.backlog                (required — epics & PBIs)

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
/speckit.fwx.docs                   (required — sprint docs)
/speckit.taskstoissues                  (optional — sync tasks to GitHub Issues)
  └─ git add + git commit               (manual — after reviewing changes)

Related Documents
─────────────────────────────────────────────
- Constitution: .specify/memory/constitution.md
- Application PRD: docs/02-agile/PRD.md
- Application SDD: docs/02-agile/SDD.md
- Application Backlog: docs/02-agile/BACKLOG.md
