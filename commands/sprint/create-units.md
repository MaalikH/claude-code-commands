---
description: Generate sprint units with stories and acceptance criteria. Creates the sprint/ directory structure for any project.
argument-hint: "<project description>" | --from <roadmap-file> | --add "<unit description>"
---

# Create Sprint Units

You are generating a structured sprint breakdown for a project. This creates the `sprint/` directory with an `index.md` and individual unit files — the canonical source of truth for the project's execution plan.

## Parse Arguments

Parse the input: `$ARGUMENTS`

Determine the mode:

1. **From scratch** (default): `$ARGUMENTS` is a project/feature description
   - Generate units from the description + any project docs found
2. **From existing**: `--from <file>` flag present
   - Convert an existing roadmap/sprint doc into standardized sprint/ format
3. **Add single unit**: `--add "<description>"` flag present
   - Append one unit to an existing sprint/ directory
4. **Empty**: No arguments
   - Look for project docs (CLAUDE.md, README, business plans, PRDs) and generate from those

## Phase 0: Project Discovery

1. **Read project context** — check for these files (read whichever exist):
   - `CLAUDE.md` — project instructions and context
   - `README.md` — project overview
   - `BUSINESS_PLAN.md`, `PRD.md`, `REQUIREMENTS.md` — product docs
   - `AGENT_ROADMAP.md` — existing agent/roadmap structure
   - `SPRINT_UNITS.md` — existing sprint breakdown
   - `sprint/index.md` — existing sprint (for --add mode)

2. **Determine project name** from CLAUDE.md or README or directory name

3. **Determine tech stack** from CLAUDE.md, package.json, or project files

Report: "Project: [name], Tech: [stack summary]"

## Phase 1: Generate Units (From Scratch)

**Skip if `--from` or `--add` mode.**

1. **Analyze the project** and break it into day-sized units:
   - Each unit = ~1 focused work session
   - Units are sequenced by dependency (what blocks what)
   - Group into phases: Foundation -> Core Build -> Integration -> QA & Launch

2. **For each unit, generate**:
   - Name (concise, action-oriented)
   - Agent assignment (optional — use "none" if not a multi-agent project)
   - Phase number
   - Dependencies (which units must complete first)
   - Parallel flag (can it run alongside other units?)
   - Priority (high/medium/low based on phase)
   - 3-10 stories with acceptance criteria
   - Input (what's needed before starting)
   - Output (what "done" looks like)
   - Done signal (binary pass/fail condition)

3. **Story quality rules**:
   - Every story must have a clear acceptance criteria
   - ACs must be testable and specific (not "works correctly" — say what "correct" means)
   - Include the file path or component when known
   - Mark P0 (must have) vs P1 (nice to have)

4. **Dependency rules**:
   - Foundation units block everything
   - Core build units should maximize parallelism
   - Integration units depend on the pieces they're connecting
   - QA units depend on integration being complete

## Phase 1b: Convert Existing (--from mode)

1. Read the source file
2. Parse its structure (units, stories, dependencies — whatever format it's in)
3. Convert each unit into the standardized format
4. Preserve all acceptance criteria, dependencies, and metadata

## Phase 1c: Add Single Unit (--add mode)

1. Read existing `sprint/index.md` to get current unit count and dependency graph
2. Generate the new unit with the next available number
3. Determine where it fits in the dependency graph
4. Generate stories with acceptance criteria

## Phase 2: Write Sprint Files

### 2a. Create sprint/ directory (if it doesn't exist)
```
sprint/
  index.md          # Summary, dependency graph, unit table
  unit-01.md        # One file per unit
  unit-02.md
  ...
```

### 2b. Write index.md

Use this YAML frontmatter format:
```yaml
---
project: <project name>
sprint: <sprint name, e.g. "MVP" or "v2">
reminders_list: <project name> Sprint
created: <today's date>
total_units: <count>
phases:
  - name: <phase name>
    units: [<unit numbers>]
---
```

Body includes:
- Dependency graph (text diagram)
- Parallel tracks (if applicable)
- Summary table of all units with status

### 2c. Write unit files

Each unit file uses this format:
```yaml
---
unit: <number>
name: <unit name>
agent: <agent or "none">
phase: <1-4>
blocked_by: [<unit numbers>]
parallel: <true/false>
priority: <high/medium/low>
status: TODO
---
```

Body format:
```markdown
# Unit XX: <Name>

## Context
<1-2 sentences about what this unit does and why>

## Stories

### Story 1: <title>
- **AC**: <acceptance criteria>
- **Priority**: P0

### Story 2: <title>
- **AC**: <acceptance criteria>
- **Priority**: P0

## Input
<what's needed before starting>

## Output
<what "done" looks like>

## Done Signal
<binary pass/fail condition>

## Docs
- <reference files to read before starting>
```

## Phase 3: Report

Display:
```
Sprint created: sprint/

Units: [count]
Phases: [count]
Total stories: [count]

| Phase | Units | Parallel Tracks |
|-------|-------|-----------------|
| ...   | ...   | ...             |

Next step: /sync-reminders to push to Apple Reminders
```

## RULES

- **Day-sized units**: Each unit should be completable in one focused session. If a unit has more than 10 stories, split it.
- **Testable ACs**: Every acceptance criteria must be verifiable. "Works correctly" is not an AC.
- **Dependency minimization**: Maximize parallelism. Only add dependencies where truly needed.
- **No tech in ACs**: ACs describe WHAT, not HOW. "User can log in" not "Supabase auth returns JWT".
- **Project-agnostic**: This format works for any project. Don't hardcode project-specific assumptions.
- **Incremental**: `--add` mode lets you grow the sprint over time without regenerating everything.
- **File per unit**: One file per unit keeps diffs clean and lets agents read only what they need.
