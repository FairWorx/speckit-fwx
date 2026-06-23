---
description: Clean up and organize raw user input into a structured requirements.md file for a greenfield FairWorx application
---

<!-- Extension: fwx -->
<!-- Config: .specify/extensions/fwx/ -->

# Generate Application Requirements

Take the user's raw description of a new FairWorx application and produce a structured `requirements.md` file under `docs/02-agile/`.

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Workflow

0. **Check project constitution**:
   - **IF EXISTS**: Load `.specify/memory/constitution.md` for project principles and governance constraints.
   - Constitution must be strictly followed.

1. **Analyze and structure the input**:
   - Read the full user input and identify the key aspects of the application being described.
   - Since this is a greenfield project, there are no existing platform docs to reference.
   - Organize the raw requirements into these sections:
     - **Application Name** — display name for the application
     - **Application Description** — 2-3 sentence summary of what the application does
     - **Core Features** — numbered list of features with clear descriptions
     - **User Personas** — who will use the application (e.g., member, admin, finance_team)
     - **Data Entities** — key database tables needed with columns and types
     - **UI/UX Requirements** — high-level UI patterns, layout preferences, design direction
     - **Technical Constraints** — any technology preferences, hosting, performance, or security requirements
     - **Open Questions** — unresolved items that need further discussion

2. **Ask for user input or confirmation**:
   - Present the inferred application name and feature list to the user for confirmation.
   - Ask clarifying questions about any ambiguous or missing areas.
   - Document their responses.

3. **Output**:
   - Write the structured content to `docs/02-agile/requirements.md`.
   - Follow markdown formatting with proper headings (##, ###).
   - Use tables where appropriate.
   - If a `docs/02-agile/requirements.md` already exists, present the user with three options:
     - **Replace** — overwrite with newly generated requirements.
     - **Update** — merge new requirements into existing.
     - **Keep existing** — abort generation.

## Requirements File Format

```markdown
# Requirements: [Application Name]

**Date:** [current date]

## Application Summary

Brief description...

## Core Features

### 1. Feature One
Description of the feature.

### 2. Feature Two
Description of the feature.

## User Personas

- **Persona 1**: Description of who they are and what they need.
- **Persona 2**: Description of who they are and what they need.

## Data Entities

### Entity Name
| Field | Type | Description |
|-------|------|-------------|

## UI/UX Requirements

- [Requirement 1]
- [Requirement 2]

## Technical Constraints

- [Constraint 1]
- [Constraint 2]

## Open Questions

- [Any unresolved questions for the developer]
```

## References

No platform documents are referenced — this command is intended for greenfield project creation.
