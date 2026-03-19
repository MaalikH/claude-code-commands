# Merge to Main

Commit all changes, push to the current branch, merge into main/master, and stay on main.

## Instructions

1. First, identify the current branch name with `git branch --show-current`
2. Identify the main branch (check if `main` or `master` exists)
3. **Run `/commit`** to stage, commit, and push all changes on the current branch
4. Switch to main/master: `git checkout main` (or `master`)
5. Pull latest: `git pull origin main`
6. Merge the feature branch: `git merge <branch-name>`
7. Push main: `git push origin main`
8. Confirm you are now on main branch and the merge was successful

## Important
- Do NOT add "Co-Authored-By" or any AI/Claude attribution
- Do NOT mention Claude, AI, or any automated tools in the commit message
- Keep the message professional and human-like
- If there are merge conflicts, stop and inform the user
- If there are no changes to commit, skip to the merge step
- Stay on main/master after completion
