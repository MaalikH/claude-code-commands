---
description: Show the current sprint status — what's done, in progress, blocked, and ready to start. Reads from sprint/ directory and Apple Reminders.
---

# Sprint Status

You are checking the current progress of the project's sprint by reading the `sprint/` directory and Apple Reminders.

This command is **project-agnostic** — it reads all context from the `sprint/` directory in the current project.

## Execution

### Step 1: Load Sprint Data

1. **Read `sprint/index.md`** — parse YAML frontmatter for:
   - `project` — project name
   - `sprint` — sprint name
   - `reminders_list` — Apple Reminders list name
   - `phases` — phase groupings
   - `total_units` — expected count

2. **Read all unit files** (`sprint/unit-*.md`) — parse YAML frontmatter for:
   - unit number, name, agent, phase, blocked_by, parallel, priority, status

### Step 2: Check Apple Reminders

1. Search the reminders list (from index.md `reminders_list` field) for all "Unit" reminders
   - Use `mcp__apple__reminders_search` with searchText "Unit" and the list name
   - Also search with `includeCompleted: true` to find completed units

2. For each unit, determine its real status:
   - **DONE**: Reminder is marked complete OR unit file has `status: DONE`
   - **READY**: All dependencies are DONE, this unit can start now
   - **BLOCKED**: Has incomplete dependencies
   - **TODO**: Not started, dependencies not yet checked (fallback)

### Step 3: Display Dashboard

```
## [Project Name] [Sprint Name] — Sprint Dashboard

Progress: [X]/[total] units complete ([Y]%)

### Phase 1 — [Phase Name]
| Unit | Name | Agent | Status | Blocked By |
|------|------|-------|--------|------------|
| 01 | ... | ... | DONE/READY/BLOCKED | ... |

### Phase 2 — [Phase Name]
[same format]

### Phase 3 — [Phase Name]
[same format]

### Phase 4 — [Phase Name]
[same format]

### Ready to Start Now
- Unit [N]: [Name] ([Agent]) — all dependencies met
- Unit [M]: [Name] ([Agent]) — all dependencies met

### Parallel Tracks
[If the index.md has parallel track info, show it with status indicators]

### Suggested Next
Based on dependency order and parallel opportunities:
1. /sprint-unit [N] — [reason]
2. /sprint-unit [M] — [can run in parallel with above]
```

### Step 4: Suggest Next Action

Recommend which unit(s) to run next:
- Prioritize `high` priority units over `medium` over `low`
- If multiple units are READY, suggest the ones that unblock the most downstream units
- Note which READY units can run in parallel

## RULES

- **Read from sprint/**: The sprint/ directory is the source of truth for structure. Reminders are the tracking layer.
- **Check both sources**: Unit file status AND Reminders completion status. If either says DONE, it's DONE.
- **List name from config**: Read `reminders_list` from `sprint/index.md`. Never hardcode.
- **Show everything**: Display all units even if the list is long. The user needs the full picture.
- **Actionable output**: Always end with a concrete `/sprint-unit <N>` suggestion.
