# Commit and Push

Commit all staged and unstaged changes with a proper commit message and push to the remote branch.

## Instructions

1. Run `git status` to see all changes (do not use -uall flag)
2. Run `git diff` to see the actual code changes
3. Run `git log -3 --oneline` to see recent commit message style
4. Analyze all changes and create a concise, descriptive commit message that:
   - Summarizes what changed (e.g., "Add paywall UI component", "Fix dark mode header", "Update execution order in ship plan")
   - Uses imperative mood (Add, Fix, Update, Remove, Refactor)
   - Is 50-72 characters for the subject line
   - Does NOT include any AI attribution or co-author tags
5. Stage all changes with `git add -A`
6. Commit with the message (use HEREDOC for proper formatting):
   ```bash
   git commit -m "Your commit message here"
   ```
7. Push to the current branch with `git push`
8. Report the commit hash and confirm the push was successful

## Important
- Do NOT add "Co-Authored-By" or any AI/Claude attribution
- Do NOT mention Claude, AI, or any automated tools in the commit message
- Keep the message professional and human-like
- If there are no changes to commit, inform the user
