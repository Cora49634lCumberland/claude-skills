# Skill Validate Command

Validates a skill's structure, metadata, and compatibility before publishing or installing.

## Usage

```
/skill validate [skill-name-or-path]
```

## Arguments

- `skill-name-or-path` (optional): Name of an installed skill or path to a local skill directory. Defaults to current directory.

## What This Command Does

1. **Locate the skill** — Finds the skill by name in installed skills or uses the provided path
2. **Check required files** — Verifies presence of `skill.json` manifest and entry point
3. **Validate manifest schema** — Ensures all required fields are present and correctly typed
4. **Check version format** — Validates semver compliance for version strings
5. **Verify dependencies** — Checks that declared dependencies exist in the marketplace or are resolvable
6. **Lint command files** — Validates `.md` command files for proper structure and frontmatter
7. **Check permissions** — Validates that declared permissions are recognized and not overly broad
8. **Test entry point** — Attempts a dry-run import/load of the skill entry point
9. **Report results** — Summarizes validation results with pass/fail status per check

## Validation Checks

### Required Manifest Fields
- `name` — Must be lowercase, alphanumeric with hyphens only
- `version` — Must follow semver (e.g., `1.0.0`)
- `description` — Non-empty string, max 200 characters
- `author` — Non-empty string
- `entry` — Path to entry point file that must exist

### Optional but Validated Fields
- `homepage` — Must be a valid URL if present
- `repository` — Must be a valid URL if present
- `license` — Should be a recognized SPDX identifier
- `keywords` — Array of strings, max 10 items
- `dependencies` — Object mapping skill names to version ranges
- `permissions` — Array of recognized permission strings

### Recognized Permissions
- `filesystem:read`
- `filesystem:write`
- `network:fetch`
- `shell:execute`
- `git:read`
- `git:write`

## Example Output

```
Validating skill: my-skill (./my-skill)

✓ skill.json found
✓ Entry point exists: index.js
✓ Manifest schema valid
✓ Version format valid: 1.2.0
✓ Description length OK (87 chars)
✓ Author field present
⚠ No license specified (recommended: MIT)
✓ Dependencies resolved (2/2)
✓ Command files valid (3 files)
✗ Unknown permission: filesystem:delete
  → Use filesystem:write instead

Result: 1 error, 1 warning
Skill is NOT ready to publish. Fix errors before proceeding.
```

## Exit Behavior

- **All checks pass** → Confirms skill is valid and ready
- **Warnings only** → Reports warnings but confirms skill is publishable
- **Errors found** → Lists all errors with suggested fixes; skill is blocked from publishing

## Related Commands

- `/skill publish` — Publish skill to marketplace (runs validate automatically)
- `/skill info` — View detailed skill information
- `/skill add` — Install a skill from marketplace
