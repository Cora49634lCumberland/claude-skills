# Remove Skill

Remove an installed skill from the claude-skills configuration.

## Usage

```
/skill/remove <skill-name>
```

## Arguments

- `skill-name` (required): The name or ID of the skill to remove

## Steps

1. **Validate the skill exists**
   - Check `.claude-plugin/marketplace.json` for the skill entry
   - Verify the skill is currently installed in the local configuration
   - If not found, display an error message with available installed skills

2. **Show skill details before removal**
   - Display the skill name, version, and description
   - List any dependencies that may be affected
   - Ask for confirmation before proceeding

3. **Remove skill files**
   - Delete the skill's command file(s) from `.claude/commands/`
   - Remove any associated configuration entries
   - Clean up any skill-specific cache or data files

4. **Update marketplace registry**
   - Mark the skill as uninstalled in `.claude-plugin/marketplace.json`
   - Update the `installed` field to `false`
   - Record the removal timestamp in the skill metadata

5. **Handle dependencies**
   - Check if other installed skills depend on this skill
   - If dependents exist, warn the user and list affected skills
   - Offer to remove dependent skills or cancel the operation

6. **Confirm removal**
   - Display a success message: `✓ Skill '<skill-name>' has been removed`
   - Show remaining installed skills count
   - Suggest related skills from the marketplace if applicable

## Example

```
/skill/remove git-flow

Removing skill: git-flow v1.2.0
Description: Advanced git workflow automation

Are you sure you want to remove this skill? (y/N): y

✓ Skill 'git-flow' has been removed successfully
You have 3 skills remaining installed.
```

## Error Handling

- If skill is not installed: `Error: Skill '<skill-name>' is not currently installed`
- If skill has active dependents: `Warning: The following skills depend on '<skill-name>': [list]`
- If removal fails due to permissions: `Error: Unable to remove skill files. Check file permissions.`
