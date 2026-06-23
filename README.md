# FairWorx Module Generator (fwx)

Speckit extension for scaffolding FairWorx platform modules following platform constitution, UI guidelines, and architectural patterns.

## Prerequisites

- [Speckit](https://opencode.ai) `>=0.2.0`
- [opencode](https://opencode.ai)

## Commands

| Command | Description |
|---------|-------------|
| `speckit.fwx.mod.prd` | Generate a Product Requirements Document (PRD.md) for a module |
| `speckit.fwx.mod.sdd` | Generate a Solution Design Document (SDD.md) for a module |
| `speckit.fwx.mod.backlog` | Generate a module-level BACKLOG.md from the PRD |
| `speckit.fwx.mod.requirements` | Clean up raw user input into a structured requirements.md |
| `speckit.fwx.mod.docs` | Generate sprint report, devlog, update as-is docs, and sync SDD after implementation |

## Usage

```
speckit run fwx.<command>
```
