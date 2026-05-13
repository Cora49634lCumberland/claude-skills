# Create Pull Request

Create a pull request with a well-structured description based on the current branch changes.

## Usage

```
/git/pr [title]
```

## Steps

1. **Gather branch info**
   - Get current branch name: `git branch --show-current`
   - Get base branch (default: `main` or `master`)
   - List commits since divergence: `git log base..HEAD --oneline`

2. **Analyze changes**
   - Summarize diff: `git diff base..HEAD --stat`
   - Identify changed files and their purpose
   - Detect breaking changes, new features, or bug fixes

3. **Generate PR description**

   Use this template:

   ```markdown
   ## Summary
   <!-- Brief description of what this PR does -->

   ## Changes
   <!-- Bullet list of key changes -->

   ## Type of Change
   - [ ] Bug fix
   - [ ] New feature
   - [ ] Breaking change
   - [ ] Documentation update
   - [ ] Refactor

   ## Testing
   <!-- How was this tested? -->

   ## Related Issues
   <!-- Link any related issues: Closes #123 -->
   ```

4. **Create the PR**
   - If `gh` CLI is available: `gh pr create --title "<title>" --body "<description>"`
   - Otherwise, output the title and description for manual submission

## Notes

- Branch name is used to infer PR title if not provided (e.g., `feat/add-auth` → "Add auth")
- Conventional commit prefixes in commits help auto-classify the change type
- Always review the generated description before submitting
