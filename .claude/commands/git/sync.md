# Git Sync Command

Keeps your current branch up to date with the main branch by fetching, rebasing, and resolving common conflicts.

## Usage

```
/git/sync [--branch <branch>] [--strategy <rebase|merge>]
```

## Arguments

- `--branch`: The upstream branch to sync with (default: `main`)
- `--strategy`: Sync strategy to use — `rebase` (default) or `merge`

## Steps

1. Stash any uncommitted changes to avoid conflicts during sync
2. Fetch the latest changes from `origin`
3. Apply the selected strategy (`rebase` or `merge`) against the upstream branch
4. Pop the stash if changes were stashed in step 1
5. Report the result — commits ahead/behind, files changed

## Examples

```bash
# Sync current branch with main using rebase (default)
/git/sync

# Sync with a different upstream branch
/git/sync --branch develop

# Sync using merge instead of rebase
/git/sync --strategy merge
```

## Notes

- If conflicts occur during rebase/merge, the command will pause and list the conflicting files
- After resolving conflicts manually, run `git rebase --continue` or `git merge --continue`
- Stashed changes are restored automatically after a successful sync
- Use `--strategy merge` if you want to preserve the branch history without rewriting commits
