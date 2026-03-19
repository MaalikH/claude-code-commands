---
description: Create or update a session handoff document for continuity
argument-hint: [list] [read]
allowed-tools: Bash, Read, Write, Edit, Grep, Glob
---

You are creating or updating a **session handoff document** — a living record of what happened in this conversation so a future session (or human) can pick up seamlessly.

## Argument Modes

- If `$ARGUMENTS` is empty: **create or update** the handoff for today + current branch.
- If `$ARGUMENTS` is `list`: **list** all handoff files for the current project, then stop.
- If `$ARGUMENTS` is `read`: **display** the current handoff document contents, then stop.

---

## Mode: `list`

1. Derive the project name from the basename of the current working directory.
2. Run `ls -la ~/.claude/handoffs/<project>/` to list all handoff files.
3. If the directory doesn't exist, report "No handoffs found for <project>."
4. Output the list and stop. Do nothing else.

## Mode: `read`

1. Derive the project name and branch (see identity steps below).
2. Construct the file path: `~/.claude/handoffs/<project>/<YYYY-MM-DD>-<branch>.md`
3. If the file exists, read it and display the full contents.
4. If it doesn't exist, report "No handoff found for today on this branch."
5. Stop. Do nothing else.

## Mode: Create / Update (default)

### Step 1 — Derive Session Identity

1. **Project name**: basename of `pwd` (e.g., `/Users/me/Dev/my-app` → `my-app`)
2. **Branch**: run `git branch --show-current`
   - If empty (detached HEAD): use the first 8 chars of `git rev-parse HEAD` as the identifier
   - If not a git repo: use `no-branch`
   - Replace all `/` with `-` in the branch name
   - Truncate to 60 characters if longer
3. **Date**: today's date as `YYYY-MM-DD`
4. **File path**: `~/.claude/handoffs/<project>/<date>-<branch>.md`

### Step 2 — Check Existing File

1. Run `mkdir -p ~/.claude/handoffs/<project>/`
2. Check if the file at the constructed path already exists.
3. If it exists:
   - Read the file.
   - Extract the content under `## Pinned Notes` (everything from that heading until the next `##` heading or end of file). Preserve this exactly — it must survive the rewrite.
   - Extract the current session count from the `**Session count**` field. Increment it by 1.
   - Note this is an **UPDATE**.
4. If it doesn't exist:
   - Session count = 1.
   - Pinned Notes = empty (just the heading with a placeholder line).
   - Note this is a **CREATE**.

### Step 3 — Gather Context

Run these git commands (skip if not a git repo):

```bash
git status
git diff --stat
git log --oneline -15
```

Also review the full conversation history to understand what was discussed, decided, and done.

### Step 4 — Write the Handoff Document

Write (or overwrite) the file at the constructed path with this exact structure:

```markdown
# Session Handoff: <project>

**Branch**: `<branch>` | **Updated**: <YYYY-MM-DD HH:MM> | **Session count**: <N>

## Summary

<!-- 2-4 sentences. Someone with ZERO context should understand what this project/session is about. -->

## What Was Done

<!-- Chronological bullet list of concrete actions taken. Be specific: "Added X to Y", "Fixed bug where Z", "Refactored A into B". -->

## Key Decisions

<!-- Each entry: **Decision** — rationale. Only include decisions that affect future work. -->

## Files Changed

| File | Status | What Changed |
|------|--------|-------------|
<!-- One row per file. Status = Added/Modified/Deleted. "What Changed" = brief description. -->

## Current State

<!-- What would someone see if they sat down right now? Is the app running? Are there failing tests? Is there a half-finished feature? Are there uncommitted changes? -->

## Next Steps

<!-- Ordered list of what to do next. Be actionable and specific. -->

## Pinned Notes

<!-- PRESERVED across updates. If updating, paste the previous Pinned Notes content here exactly. If creating, add this placeholder: -->
<!-- Add persistent notes here. This section is preserved across handoff updates. -->
```

### Step 5 — Report to User

Output a brief confirmation:

```
<CREATE or UPDATE> handoff: ~/.claude/handoffs/<project>/<filename>
Session count: <N>
Summary: <1-line summary of what the handoff captures>
```

### Step 6 — Show Brief

After the handoff is written, immediately output a concise bullet list summarizing the changes made during this conversation — one line per change, plain language, no code blocks. Group by theme if there are many changes. This gives the user a quick recap of what the handoff captured.

Format it under a `**Changes this session:**` header so it's visually distinct from the handoff confirmation above.

## Important Rules

- The `## Pinned Notes` section is SACRED. When updating, always preserve its content exactly as-is from the previous version. Never modify, summarize, or remove anything in Pinned Notes.
- Be specific and concrete in all sections. Avoid vague statements like "made improvements" or "updated code".
- The Summary must be understandable by someone with zero context about the project.
- Files Changed should reflect what actually changed on disk (cross-reference with git), not just what was discussed.
- Keep the document scannable — use bullets, tables, and short sentences.
- Do NOT include any AI attribution or co-author tags.
