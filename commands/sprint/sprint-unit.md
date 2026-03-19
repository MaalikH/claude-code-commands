---
description: Execute a sprint unit by running /build-feature-speckit for each story. Reads from sprint/ directory and validates dependencies via Apple Reminders.
argument-hint: <unit-number> [--dry-run] [--skip-review] [--story <story-index>]
---

# Sprint Unit Executor

You are the **Sprint Orchestrator**. Your job is to execute a single sprint unit by running the full SpecKit workflow for each user story in that unit, in the correct order, with the correct agent context.

This command is **project-agnostic** — it reads all context from the `sprint/` directory in the current project.

## Parse Arguments

Parse the input: `$ARGUMENTS`

Extract:
1. **Unit number**: Required. The sprint unit to execute.
2. **Flags**:
   - `--dry-run`: Show what would be executed without actually running anything
   - `--skip-review`: Pass through to `/build-feature-speckit` (skip code review phases)
   - `--story <N>`: Only execute story N (1-indexed) from this unit, not all stories

If no unit number is provided, ERROR: "Usage: /sprint-unit <unit-number> [--dry-run] [--skip-review] [--story <N>]"

## Phase 0: Load Unit Context

1. **Read `sprint/index.md`** — parse YAML frontmatter for:
   - `project` — project name
   - `reminders_list` — Apple Reminders list name
   - `phases` — phase groupings
   - Dependency graph from body

2. **Read the unit file** `sprint/unit-XX.md` (zero-padded):
   - Parse YAML frontmatter: unit, name, agent, phase, blocked_by, parallel, priority, status
   - Parse body: stories (title + AC + priority), input, output, done signal, docs
   - If the file has an `## Integration Handoffs` section, capture those too

3. **Read project CLAUDE.md** (if exists) for:
   - Tech stack
   - Development rules (e.g., "no Tailwind")
   - Any project-specific constraints

4. **Build the agent context package**:
   ```
   Project: [project name]
   Agent: [agent from unit frontmatter, or "none"]
   Phase: [phase number]
   Unit: [unit number] - [unit name]
   ```

Report: "Phase 0 Complete - Unit [N]: [Name] loaded. [X] stories to execute."

## Phase 1: Dependency Check

**CRITICAL: Do not proceed if dependencies are unmet.**

1. Read `blocked_by` from unit frontmatter
2. If empty array `[]` — proceed immediately
3. If has dependencies:
   - Read `reminders_list` from `sprint/index.md` frontmatter
   - For each dependency unit number:
     - Search Apple Reminders in the list for "Unit XX" (with `includeCompleted: true`)
     - Check if the dependency reminder is marked as completed
   - **If ANY dependency is incomplete**: STOP and report:
     ```
     BLOCKED: Unit [N] requires these units to be completed first:
     - Unit [X]: [Name] - [INCOMPLETE/COMPLETE]
     - Unit [Y]: [Name] - [INCOMPLETE/COMPLETE]

     Complete the blocking units first, then re-run /sprint-unit [N].
     ```
   - **If ALL dependencies are complete**: Proceed

Report: "Phase 1 Complete - All dependencies satisfied."

## Phase 2: Story Execution Plan

1. List all stories for this unit with their acceptance criteria (from the unit file)
2. Filter to P0 stories first, then P1 (unless `--story` targets a specific one)
3. If `--story <N>` flag is set, only plan that single story
4. For each story, prepare the feature description for `/build-feature-speckit`:

   **Feature description format:**
   ```
   [SPRINT UNIT {unit_number} - STORY {story_index}]

   Project: {project name from index.md}
   Agent: {agent from unit frontmatter}
   Phase: {phase number}

   Feature: {story title}

   Acceptance Criteria:
   {AC from unit file}

   Technical Context:
   {tech stack and constraints from CLAUDE.md, or "See project CLAUDE.md"}

   Reference Docs: {docs list from unit file}
   ```

5. **If `--dry-run`**: Display the full execution plan and STOP.

Report: "Phase 2 Complete - Execution plan ready. [X] stories queued."

## Phase 3: Execute Stories

For each story in the execution plan:

### 3a. Pre-Story Setup
- Report: "Starting Story [index]/[total]: {story title}"
- Read any reference docs listed in the unit's `## Docs` section that haven't been read yet

### 3b. Run SpecKit Workflow
- Use the **Skill** tool to invoke `build-feature-speckit` with:
  - The prepared feature description from Phase 2
  - `--skip-stories` flag (stories are already defined by our acceptance criteria)
  - `--skip-review` flag if the user passed it to `/sprint-unit`

### 3c. Post-Story Verification
- Verify the story's acceptance criteria are met by reviewing the implementation
- If the story implementation is incomplete or incorrect:
  - Report what's missing
  - Attempt to fix inline
  - If cannot fix, report and continue to next story

### 3d. Story Completion
- Report: "Story [index]/[total] Complete: {story title}"
- Track completion status

## Phase 4: Unit Completion

After all stories are executed:

1. **Verify unit output criteria**:
   - Read the `## Output` and `## Done Signal` from the unit file
   - Verify each condition is met
   - Report any gaps

2. **Update unit status**:
   - Edit the unit file's YAML frontmatter: set `status: DONE`
   - Update `sprint/index.md` status column for this unit

3. **Mark reminder as complete**:
   - Read `reminders_list` from `sprint/index.md`
   - Use `mcp__apple__reminders_complete` to mark the unit's reminder as done

4. **Generate unit summary**:
   ```
   ## Unit [N]: [Name] - COMPLETE

   Stories completed: [X]/[Y]

   | # | Story | Status |
   |---|-------|--------|
   | 1 | [story name] | DONE/PARTIAL/FAILED |
   ...

   Output verification:
   - [condition]: PASS/FAIL

   Done signal: [met/not met]

   Next units unblocked: [list]
   ```

5. **Identify next units**:
   - Scan all `sprint/unit-*.md` files for units whose `blocked_by` includes this unit number
   - Report which units are now unblocked

Report: "Phase 4 Complete - Unit [N] finished. [list of newly unblocked units]"

## IMPORTANT RULES

- **READ FROM sprint/**: The `sprint/` directory is the source of truth. Read unit files, not hardcoded docs.
- **DEPENDENCY ENFORCEMENT**: Never execute a unit whose dependencies are incomplete. Check Apple Reminders.
- **AGENT CONTEXT**: Include the agent assignment in every feature description (if not "none").
- **SEQUENTIAL STORIES**: Stories within a unit run one at a time, in order. Do not parallelize.
- **FULLY AUTONOMOUS**: Do not ask the user questions. Resolve ambiguities using project docs.
- **MARK COMPLETE**: Update both the unit file status AND the Apple Reminder when done.
- **DRY RUN**: If `--dry-run` is set, show the full plan but execute nothing.
- **SINGLE STORY**: If `--story <N>` is set, only execute that one story from the unit.
- **REFERENCE DOCS**: Read docs listed in the unit file's Docs section before starting.
- **PROJECT RULES**: Read CLAUDE.md for project-specific constraints (CSS framework, etc.) and include them in every feature description.

## Confirm Execution

Before starting, confirm to the user:

"Executing **Unit [N]: [Name]**

Project: [name]
Agent: [agent info]
Phase: [phase]
Dependencies: [all satisfied / BLOCKED]
Stories: [count] ([P0 count] P0, [P1 count] P1)
Mode: [full / dry-run / single story]

Starting now."

Then proceed to execute each phase sequentially.
