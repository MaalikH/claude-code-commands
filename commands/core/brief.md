---
description: Briefly summarize changes from this conversation
argument-hint: [last] [N]
allowed-tools: Bash, Read, Grep, Glob
---

You are summarizing the changes made during this conversation.

## Scoping

- If `$ARGUMENTS` is empty: summarize **all** changes made in this conversation.
- If `$ARGUMENTS` is `last`: summarize only the **most recent** task/exchange.
- If `$ARGUMENTS` is `last N` (e.g., `last 3`): summarize the **last N** tasks/exchanges.

## Steps

1. Review the conversation history to identify all changes Claude made (file edits, file creations, commands run, etc.), respecting the scope above.
2. Run `git status` and `git diff` to cross-reference actual file changes on disk with what was done in the conversation.
3. Produce the output below.

## Output Format

Output a **concise bullet list** — one line per change, plain language, no code blocks. Group by theme if there are many changes. Keep it scannable.

Example:
- Added user authentication middleware to the Express server
- Created login and signup page components
- Updated the route config to include auth-protected routes
- Fixed a bug where sessions weren't persisting across refreshes
