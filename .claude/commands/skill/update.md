# Skill Update Command

Update an existing skill to its latest version from the marketplace.

## Usage

```
/skill/update [skill-name]
```

## Arguments

- `skill-name` (optional): The name of the skill to update. If omitted, updates all installed skills.

## What This Command Does

1. Reads the current installed skills from `.claude-plugin/marketplace.json`
2. Fetches the latest version metadata for the specified skill(s)
3. Compares installed version against the latest available version
4. If an update is available:
   - Backs up the current skill configuration
   - Downloads and applies the updated skill definition
   - Updates the version record in `marketplace.json`
5. Reports the update status for each skill

## Steps

1. Parse `$ARGUMENTS` to determine which skill(s) to update
2. Read `.claude-plugin/marketplace.json` to get the list of installed skills and their current versions
3. For each target skill:
   a. Look up the skill entry in the marketplace registry
   b. Compare `installed_version` with `latest_version`
   c. If `installed_version < latest_version`, proceed with update
   d. Show a diff summary of what changed between versions
   e. Ask user to confirm before applying the update
4. Apply updates and write back to `marketplace.json`
5. Print a summary table showing:
   - Skill name
   - Previous version
   - New version
   - Status (updated / already up-to-date / failed)

## Example Output

```
Checking for skill updates...

┌─────────────────────┬──────────┬──────────┬─────────────────┐
│ Skill               │ Current  │ Latest   │ Status          │
├─────────────────────┼──────────┼──────────┼─────────────────┤
│ git-helper          │ 1.2.0    │ 1.3.1    │ ✓ Updated       │
│ code-reviewer       │ 2.0.0    │ 2.0.0    │ ✓ Up to date    │
│ test-generator      │ 0.9.1    │ 1.0.0    │ ✓ Updated       │
└─────────────────────┴──────────┴──────────┴─────────────────┘

2 skill(s) updated successfully.
```

## Error Handling

- If a skill name is not found in the marketplace, display an error and skip it
- If the marketplace registry is unreachable, fall back to cached metadata
- If an update fails mid-way, restore the backup and report the failure
- Validate skill integrity after update using checksum verification
