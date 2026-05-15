# Skill Migrate Command

Migrate skills from one version or format to another, handling breaking changes and schema updates.

## Usage

```
/skill migrate [skill-name] [--from <version>] [--to <version>] [--dry-run] [--all]
```

## Arguments

- `skill-name` - Name of the skill to migrate (optional if `--all` is used)
- `--from <version>` - Source version to migrate from (auto-detected if omitted)
- `--to <version>` - Target version to migrate to (defaults to latest)
- `--dry-run` - Preview migration changes without applying them
- `--all` - Migrate all installed skills that have pending migrations
- `--backup` - Create a backup before migrating (default: true)
- `--force` - Skip confirmation prompts

## Process

1. **Detect current version** of the skill(s) to migrate
2. **Check marketplace** for available migration paths
3. **Validate migration path** exists between source and target versions
4. **Show diff** of changes that will be applied
5. **Create backup** of current skill configuration (unless `--no-backup`)
6. **Apply migrations** sequentially if multiple version jumps are needed
7. **Verify integrity** of migrated skill
8. **Update skill registry** with new version metadata

## Migration Steps

For each skill being migrated:

```
Checking skill: <skill-name>
  Current version: <from-version>
  Target version:  <to-version>
  Migration path:  v1.0.0 → v1.1.0 → v2.0.0

Changes to apply:
  [+] New configuration keys: timeout, retry_count
  [~] Renamed: api_key → auth_token
  [-] Removed deprecated: legacy_mode

Proceed with migration? [y/N]
```

## Backup Behavior

Backups are stored in `.claude/skills/backups/<skill-name>-<timestamp>/` and contain:
- Original skill files
- Configuration snapshot
- Rollback instructions

## Rollback

If migration fails or produces unexpected results:

```
/skill migrate <skill-name> --rollback
```

This restores the most recent backup for the specified skill.

## Examples

```bash
# Migrate a specific skill to latest version
/skill migrate github-tools

# Preview migration without applying
/skill migrate github-tools --dry-run

# Migrate all skills with pending updates
/skill migrate --all

# Migrate to a specific version
/skill migrate github-tools --to 2.1.0

# Migrate without creating a backup (not recommended)
/skill migrate github-tools --no-backup --force
```

## Error Handling

- If no migration path exists between versions, display available paths
- If a migration step fails, automatically attempt rollback to last known good state
- Log all migration operations to `.claude/claudex/log` for audit trail

## Notes

- Always review the dry-run output before applying migrations in production
- Major version migrations (e.g., v1.x → v2.x) may require manual configuration updates
- Skills with custom modifications will show a warning before migration proceeds
