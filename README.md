# FairWorx Module Generator (fwx)

Speckit extension for scaffolding FairWorx platform modules following platform constitution, UI guidelines, and architectural patterns.

## Prerequisites

- [Speckit](https://opencode.ai) `>=0.2.0`
- [opencode](https://opencode.ai)

## Commands

### Application-Level

| Command | Description |
|---------|-------------|
| `speckit.fwx.init` | Initialize docs structure — verify commands, create folders, seed baseline docs from templates |
| `speckit.fwx.requirements` | Organize raw user input into a structured `requirements.md` for a greenfield application |
| `speckit.fwx.prd` | Generate a Product Requirements Document (PRD.md) using the app PRD template |
| `speckit.fwx.sdd` | Generate a Solution Design Document (SDD.md) using the app SDD template |
| `speckit.fwx.backlog` | Generate an application-level BACKLOG.md by breaking down the PRD into epics and PBIs |
| `speckit.fwx.docs` | Generate sprint report, devlog, update as-is docs, and sync SDD after implementation |

### Module-Level

| Command | Description |
|---------|-------------|
| `speckit.fwx.mod-requirements` | Organize raw user input into a structured `requirements.md` for a new module |
| `speckit.fwx.mod-prd` | Generate a Product Requirements Document (PRD.md) for a module |
| `speckit.fwx.mod-sdd` | Generate a Solution Design Document (SDD.md) for a module |
| `speckit.fwx.mod-backlog` | Generate a module-level BACKLOG.md by breaking down the PRD into epics and PBIs |

## Usage

```
speckit run fwx.<command>
```
