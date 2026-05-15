# Skill Export Command

Export one or more installed skills to a portable format for sharing or backup.

## Usage

```
/skill/export [skill-name] [options]
```

## Arguments

- `skill-name` - Name of the skill to export (optional, exports all if omitted)
- `--output <path>` - Output directory or file path (default: `./exports`)
- `--format <format>` - Export format: `zip`, `json`, `tar` (default: `zip`)
- `--include-deps` - Include skill dependencies in the export
- `--overwrite` - Overwrite existing export files

## Examples

```
/skill/export my-skill
/skill/export my-skill --output ./backups --format json
/skill/export --include-deps
/skill/export my-skill --output ~/Desktop/my-skill-backup.zip
```

## Behavior

1. **Validate skill exists** — Check that the specified skill is installed in `.claude/skills/`
2. **Resolve dependencies** — If `--include-deps` is set, gather all dependent skills
3. **Bundle skill files** — Package the skill manifest, commands, and assets
4. **Write export** — Save to the specified output path in the chosen format
5. **Confirm export** — Display the export path and file size

## Export Format

### JSON format
```json
{
  "exported_at": "2024-01-15T10:30:00Z",
  "claude_skills_version": "1.0.0",
  "skills": [
    {
      "name": "skill-name",
      "version": "1.2.3",
      "manifest": { ... },
      "files": { "path/to/file": "base64-encoded-content" }
    }
  ]
}
```

### ZIP / TAR format
```
export-20240115/
  manifest.json          # Export metadata
  skills/
    skill-name/
      skill.json         # Skill manifest
      commands/          # Skill command files
      assets/            # Any additional assets
```

## Notes

- Exported skills can be re-imported using `/skill/add <export-path>`
- Credentials and secrets are **never** included in exports
- Large binary assets are excluded by default; use `--include-assets` to override
- Export files are named `<skill-name>-<version>-<timestamp>.<ext>` by default

## Error Handling

- If the skill is not found, suggest running `/skill/list` to see available skills
- If the output path is not writable, display a clear permission error
- If an export already exists at the target path, prompt to overwrite unless `--overwrite` is set
