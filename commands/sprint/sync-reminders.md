---
description: Sync sprint/ directory to Apple Reminders. Creates a list and one reminder per unit with stories and acceptance criteria.
argument-hint: [--clean] [--update <unit-number>]
---

# Sync Sprint to Apple Reminders

You are syncing the project's `sprint/` directory to Apple Reminders so units can be tracked, assigned, and checked off.

## Parse Arguments

Parse the input: `$ARGUMENTS`

Flags:
- `--clean`: Delete the existing Reminders list and recreate from scratch
- `--update <N>`: Only update the reminder for unit N (don't touch others)
- No flags: Create list if missing, add any units that don't have reminders yet

## Phase 0: Read Sprint Data

1. **Read `sprint/index.md`** — parse the YAML frontmatter for:
   - `project` — project name
   - `sprint` — sprint name
   - `reminders_list` — the Apple Reminders list name to use
   - `phases` — phase groupings
   - `total_units` — expected unit count

2. **Read all unit files** (`sprint/unit-*.md`) — parse each for:
   - YAML frontmatter: unit number, name, agent, phase, blocked_by, parallel, priority, status
   - Body: stories with ACs, input, output, done signal, docs

3. **Validate**:
   - All unit files referenced in index.md exist
   - No duplicate unit numbers
   - Dependencies reference valid unit numbers

Report: "Sprint loaded: [project] [sprint] — [N] units across [P] phases"

## Phase 1: Prepare Reminders List

1. **If `--clean`**: Ask user to manually delete the existing list (Apple Reminders API has no delete). Then create a new list.

2. **Otherwise**: Search for the list by name
   - If it exists: proceed (will add missing reminders)
   - If it doesn't exist: create it with `mcp__apple__reminders_create_list`

## Phase 2: Create/Update Reminders

For each unit (or just the specified unit if `--update`):

### 2a. Build reminder title
Format: `Unit XX: <Name> — <Agent>`

### 2b. Build reminder notes
```
PHASE: <phase number>
AGENT: <agent assignment>
BLOCKED BY: <comma-separated unit numbers, or "None">
PARALLEL: <Yes/No>
PRIORITY: P0

STORIES:
- [ ] <Story 1 title>
  -> AC: <acceptance criteria>
- [ ] <Story 2 title>
  -> AC: <acceptance criteria>
...

INPUT: <what's needed>
OUTPUT: <what done looks like>
DONE SIGNAL: <binary condition>

DOCS: <reference files>
PROJECT: <absolute path to project root>
```

### 2c. Map priority
- `high` -> Apple Reminders priority `high`
- `medium` -> Apple Reminders priority `medium`
- `low` -> Apple Reminders priority `low`

### 2d. Create the reminder
Use `mcp__apple__reminders_create` with:
- `name`: the title
- `notes`: the formatted notes
- `priority`: mapped priority
- `listName`: from index.md frontmatter

**Important**: Create reminders sequentially (not in huge parallel batches) to avoid AppleScript errors. Batches of 5-6 at a time are safe.

## Phase 3: Verify

1. Search the Reminders list for all "Unit" reminders
2. Count them and compare to expected total
3. Report any missing units

## Phase 4: Report

```
Synced to Apple Reminders: "<list name>"

| Status | Count |
|--------|-------|
| Created | [N] |
| Skipped (exists) | [N] |
| Updated | [N] |
| Total | [N] |

Open Reminders to verify: /sync-reminders wrote [N] reminders to "[list name]"
```

Open the Reminders app to the list when done.

## RULES

- **Read from sprint/**: The sprint/ directory is the source of truth. Reminders are a projection.
- **Don't duplicate**: Before creating a reminder, check if one with that title already exists in the list. Skip if it does (unless --update).
- **Sequential creation**: Create reminders in batches of 5-6 max to avoid AppleScript timeouts.
- **Use -> not arrow unicode**: AppleScript chokes on unicode arrows. Use `->` for AC markers.
- **List name from config**: Always read the `reminders_list` field from `sprint/index.md`. Never hardcode.
- **Preserve completed**: When using --update, do NOT recreate already-completed reminders.
