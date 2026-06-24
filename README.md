# FairWorx Module Generator (fwx)

Speckit extension for scaffolding FairWorx platform modules following platform constitution, UI guidelines, and architectural patterns.

## Prerequisites

- [Python 3.11+](https://www.python.org/downloads/)
- [uv](https://docs.astral.sh/uv/) (recommended) or [pipx](https://pipx.pypa.io/) for persistent installation
- [Git](https://git-scm.com/downloads)
- A coding agent: [OpenCode](https://opencode.ai), [Claude Code](https://www.anthropic.com/claude-code), [GitHub Copilot](https://code.visualstudio.com/), [Gemini CLI](https://github.com/google-gemini/gemini-cli), or [Codebuddy CLI](https://www.codebuddy.ai/cli)

### Install Speckit (Persistent Installation)

Install the Speckit CLI once and use it across all projects:

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z
```

Replace `vX.Y.Z` with the [latest release tag](https://github.com/github/spec-kit/releases).

Verify installation:

```bash
specify version
```

### Initialize a Project with Speckit

**New project:**

```bash
specify init <PROJECT_NAME> --integration <agent>
```

**Existing project:**

```bash
specify init . --integration <agent>
```

The `--integration` flag selects your coding agent. Supported values:

| Agent | Example |
|-------|---------|
| OpenCode | `specify init . --integration copilot` (OpenCode auto-detects speckit commands) |
| Claude Code | `specify init . --integration claude` |
| Gemini CLI | `specify init . --integration gemini` |
| GitHub Copilot | `specify init . --integration copilot` |
| Codebuddy CLI | `specify init . --integration codebuddy` |

> **Note:** OpenCode natively supports the Speckit protocol. After running `specify init`, OpenCode automatically discovers and registers all speckit commands without additional configuration.

`specify init` creates the `.specify/` directory with scripts, templates, and memory files. For OpenCode, it also generates `.opencode/openagent.json` if it does not already exist.

## Installation

### Install the fwx Extension

Once Speckit is installed, add the `fwx` extension:

```bash
specify extension add fwx --from https://github.com/FairWorx/speckit-fwx/archive/refs/tags/v0.x.x.zip
```

Replace `v0.x.x` with the [latest release version](https://github.com/FairWorx/speckit-fwx/releases).

### Get the Latest Release

1. Go to [github.com/FairWorx/speckit-fwx/releases](https://github.com/FairWorx/speckit-fwx/releases)
2. Find the latest release (e.g., `v0.1.0`)
3. Copy the download URL and replace `v0.x.x` in the command above

To check for new releases from the CLI:

```bash
specify self check
```

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

```bash
speckit run fwx.<command>
```
