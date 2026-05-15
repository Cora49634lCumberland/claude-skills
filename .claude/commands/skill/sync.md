# Skill Sync Command

Synchronize locally installed skills with the remote marketplace, updating outdated skills and removing ones that are no longer available.

## Usage

```
/skill sync [--dry-run] [--force] [--skip <skill-name>]
```

## Arguments

- `--dry-run` — Preview what would be updated without making changes
- `--force` — Force update all skills, even if versions match
- `--skip <skill-name>` — Skip syncing a specific skill (can be repeated)

## Steps

1. **Load local skills** — Read the current skill registry from `.claude-plugin/marketplace.json` and any locally installed skill manifests

2. **Fetch remote index** — Pull the latest skill index from the upstream marketplace registry

3. **Diff versions** — Compare local skill versions against remote versions using semver comparison

4. **Identify changes**:
   - `outdated` — Local version is behind remote
   - `ahead` — Local version is newer than remote (custom/dev skills)
   - `removed` — Skill no longer exists in remote marketplace
   - `up-to-date` — Versions match

5. **Report status** — Display a summary table:
   ```
   Skill                  Local     Remote    Status
   ─────────────────────────────────────────────────
   git-helper             1.2.0     1.3.1     outdated
   code-review            2.0.0     2.0.0     up-to-date
   legacy-formatter       0.9.0     —         removed
   my-custom-skill        3.0.0-dev 2.1.0     ahead
   ```

6. **Prompt for confirmation** — Unless `--dry-run`, ask user to confirm before applying changes:
   ```
   3 skills will be updated, 1 will be flagged as removed.
   Proceed? [y/N]
   ```

7. **Apply updates** — For each outdated skill:
   - Back up current skill files to `.claude-plugin/.backup/<skill>/<version>/`
   - Download updated skill package from marketplace
   - Validate the downloaded package (checksum + schema)
   - Replace skill files atomically
   - Update version entry in local registry

8. **Handle removed skills** — For skills flagged as removed:
   - Do NOT auto-remove; warn the user instead
   - Log a notice: `⚠ 'legacy-formatter' is no longer in the marketplace. Run /skill remove legacy-formatter to uninstall.`

9. **Skip ahead skills** — Skills with local versions newer than remote are skipped with an info message:
   - `ℹ 'my-custom-skill' is ahead of marketplace (3.0.0-dev > 2.1.0). Skipping.`

10. **Finalize** — Write updated registry, display completion summary:
    ```
    ✓ Sync complete.
      Updated : 3 skills
      Skipped : 1 skill (ahead of remote)
      Warnings: 1 skill removed from marketplace
    ```

## Error Handling

- If remote index is unreachable, abort with: `✗ Cannot reach marketplace. Check your connection and try again.`
- If a skill update fails validation, roll back from backup and continue with remaining skills
- If backup directory already exists for a version, skip backup (idempotent)

## Notes

- Skills installed from local paths (prefixed with `file://`) are never synced against remote
- Use `/skill update <name>` to update a single skill manually
- Backups older than 30 days are automatically pruned during sync
