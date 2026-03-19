---
description: Detailed breakdown of changes from this conversation
argument-hint: [last] [N]
allowed-tools: Bash, Read, Grep, Glob
---

You are producing a detailed breakdown of the changes made during this conversation.

## Scoping

- If `$ARGUMENTS` is empty: cover **all** changes made in this conversation.
- If `$ARGUMENTS` is `last`: cover only the **most recent** task/exchange.
- If `$ARGUMENTS` is `last N` (e.g., `last 2`): cover the **last N** tasks/exchanges.

## Steps

1. Review the conversation history to identify all changes Claude made (file edits, file creations, commands run, etc.), respecting the scope above.
2. Run `git status` and `git diff` to get the actual current state of changes on disk.
3. For each changed file, read the relevant sections if needed to provide accurate code context.
4. Produce the output below.

## Output Format

Provide a **per-file breakdown** with the following structure:

### For each file changed:

**`path/to/file`** — one-line summary of what changed

- **What**: describe the specific changes (new functions, modified logic, config updates, etc.)
- **Why**: explain the reasoning or motivation behind the change
- **Key changes**: show short, relevant code snippets illustrating the most important modifications

### At the end:

**How changes connect**: a short paragraph explaining how the individual file changes work together to achieve the overall goal.

Keep code snippets focused — show only the lines that matter, not entire files.
