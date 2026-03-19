# Claude Code Slash Commands

33 custom slash commands I use every day with Claude Code. Drop them into your `.claude/commands/` folder and they just work.

## Quick Start

**Use all commands (global):**
```bash
git clone https://github.com/MaalikH/claude-code-commands.git
cp -r claude-code-commands/commands/* ~/.claude/commands/
```

**Use all commands (project level):**
```bash
git clone https://github.com/MaalikH/claude-code-commands.git
cp -r claude-code-commands/commands/* .claude/commands/
```

**Pick and choose:**
```bash
# Just grab the ones you want
cp claude-code-commands/commands/core/commit.md ~/.claude/commands/
cp claude-code-commands/commands/core/review.md ~/.claude/commands/
```

## Commands

### Core (16 commands)

| Command | What it does |
|---------|-------------|
| `/commit` | Stages, diffs, writes a commit message matching your repo style, pushes. You never write a commit message again. |
| `/build-feature` | One sentence to a fully implemented feature. 9 phases: intake, stories, spec, clarification, planning, tasks, implementation, review, summary. |
| `/build-feature-speckit` | Same as /build-feature but uses the SpecKit workflow for spec driven development. |
| `/review` | Senior engineer code review before you commit. Security, performance, edge cases, type safety. |
| `/cleanup` | Strips console.logs, removes commented out code, fixes formatting. Production readiness pass. |
| `/handoff` | Writes a session handoff doc so the next conversation picks up where this one left off. |
| `/merge` | Handles the full merge to main workflow. |
| `/explain` | Deep explanation of code, concepts, or changes with context and reasoning. |
| `/brief` | Quick summary of changes from the current conversation. |
| `/detailed` | Detailed breakdown of all changes from the current conversation. |
| `/walkthrough` | Interactive code walkthrough. |
| `/architecture` | Generates or reviews project architecture. |
| `/extract-components` | Scans code for repeated UI patterns and refactors them into reusable components. |
| `/run-device` | Builds, installs, and launches an iOS app on a connected physical device. One command. |
| `/create-spec` | Creates a product specification from requirements. |
| `/create-stories` | Generates user stories with acceptance criteria. |

### SpecKit (9 commands)

A spec driven development workflow. Each command handles one phase.

| Command | What it does |
|---------|-------------|
| `/speckit.specify` | Creates or updates a feature specification from a description. |
| `/speckit.clarify` | Identifies underspecified areas and asks up to 5 targeted questions. |
| `/speckit.plan` | Generates an implementation plan from the spec. |
| `/speckit.tasks` | Creates dependency ordered, actionable tasks from the plan. |
| `/speckit.implement` | Executes all tasks defined in the plan. |
| `/speckit.analyze` | Cross artifact consistency check across spec, plan, and tasks. |
| `/speckit.checklist` | Generates a custom checklist for the current feature. |
| `/speckit.constitution` | Creates or updates the project constitution (design principles). |
| `/speckit.taskstoissues` | Converts tasks into GitHub issues. |

### QA (4 commands)

Browser testing through Chrome automation.

| Command | What it does |
|---------|-------------|
| `/qa.run` | Runs browser tests and produces a pass/fail report. |
| `/qa.create` | Generates browser test scripts from a description or test inventory. |
| `/qa.scan` | Scans the codebase to discover all testable routes, pages, and endpoints. |
| `/qa.report` | Reads the last test report and shows a clean summary with regression trends. |

### Sprint (4 commands)

Sprint management integrated with Apple Reminders.

| Command | What it does |
|---------|-------------|
| `/sprint-unit` | Executes an entire sprint unit. Reads stories and builds each one. |
| `/sprint-status` | Shows what's done, in progress, blocked, and ready to start. |
| `/sync-reminders` | Syncs sprint stories to Apple Reminders so you can check progress from your phone. |
| `/create-units` | Creates sprint units from a backlog. |

## How Slash Commands Work

Claude Code looks for `.md` files in two places:

- **Project level:** `.claude/commands/` in your repo
- **Global:** `~/.claude/commands/` on your machine

The filename becomes the command. `commit.md` becomes `/commit`. Inside the file is a prompt that tells Claude what to do when you run it. Use `$ARGUMENTS` to capture whatever the user types after the command.

## Structure

```
commands/
  core/         # Daily workflow commands
  speckit/      # Spec driven development
  qa/           # Browser testing
  sprint/       # Sprint management
```

## License

MIT
