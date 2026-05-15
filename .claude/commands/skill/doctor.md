# Skill Doctor

Diagnose and fix issues with installed skills in your Claude project.

## Usage

```
/skill/doctor [skill-name]
```

## Arguments

- `skill-name` (optional): Name of a specific skill to diagnose. If omitted, runs diagnostics on all installed skills.

## What It Checks

### For Each Skill

1. **Manifest Integrity** - Validates the skill's manifest file exists and is valid JSON/YAML
2. **Dependency Resolution** - Checks all declared dependencies are installed and at compatible versions
3. **Command File Presence** - Verifies all commands declared in the manifest have corresponding `.md` files
4. **Marketplace Sync** - Compares installed version against latest in marketplace
5. **Broken References** - Scans command files for references to missing skills or commands
6. **Circular Dependencies** - Detects circular dependency chains that could cause issues

## Behavior

1. Read `.claude-plugin/marketplace.json` to get skill registry
2. Scan installed skills from project configuration
3. Run each diagnostic check and collect results
4. Report issues grouped by severity: ERROR, WARNING, INFO
5. For auto-fixable issues, prompt user to apply fixes
6. Generate a health report summary

## Output Format

```
Running skill diagnostics...

✓ skill-name (v1.2.0)
  ✓ Manifest valid
  ✓ All 3 dependencies resolved
  ✓ All command files present
  ⚠ Update available: v1.3.0
  ✓ No broken references

✗ another-skill (v0.9.1)
  ✓ Manifest valid
  ✗ Missing dependency: base-utils@^2.0.0 (installed: 1.8.3)
  ✗ Missing command file: .claude/commands/another-skill/helper.md
  ✓ No circular dependencies

Summary: 1 healthy, 1 needs attention
Errors: 2 | Warnings: 1 | Info: 0

Auto-fixable issues found. Run `/skill/doctor --fix` to attempt automatic repairs.
```

## Flags

- `--fix`: Attempt to automatically repair fixable issues (re-install missing deps, restore missing command files from marketplace)
- `--verbose`: Show detailed output for each check, including passing checks
- `--json`: Output results as JSON for programmatic use
- `--no-update-check`: Skip marketplace version comparison (useful offline)

## Auto-Fix Capabilities

The `--fix` flag can automatically resolve:
- Missing or outdated dependencies (runs equivalent of `/skill/add` for each)
- Skills that have drifted from their marketplace version (prompts to re-sync)

The `--fix` flag **cannot** automatically resolve:
- Circular dependencies (requires manual intervention)
- Custom modifications that conflict with upstream changes
- Skills removed from the marketplace

## Examples

```bash
# Check all skills
/skill/doctor

# Check a specific skill
/skill/doctor git-helpers

# Check and auto-fix issues
/skill/doctor --fix

# Verbose output for debugging
/skill/doctor git-helpers --verbose

# Get JSON report for CI integration
/skill/doctor --json > skill-health-report.json
```

## Exit Behavior

- If any ERRORs are found, the command exits with a non-zero status
- WARNINGs and INFO messages do not affect exit status
- Use `--fix` to resolve errors before committing or deploying
