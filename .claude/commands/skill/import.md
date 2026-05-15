# Skill Import Command

Import a skill from a local file, directory, or remote URL into your Claude skills collection.

## Usage

```
/skill/import <source> [--name <skill-name>] [--force] [--dry-run]
```

## Arguments

- `source` - Path to a local `.md` file, directory containing skill files, or a remote URL (GitHub raw URL, gist, etc.)
- `--name` - Override the skill name derived from the file (optional)
- `--force` - Overwrite existing skill if it already exists
- `--dry-run` - Preview what would be imported without making changes

## Steps

1. **Resolve the source**
   - If source is a URL (starts with `http://` or `https://`), fetch the content
   - If source is a local path, read the file or directory
   - Validate the source is accessible and readable

2. **Parse skill content**
   - Read the markdown file(s)
   - Extract the skill name from the filename or `--name` flag
   - Validate the skill structure (must have a `#` heading and description)

3. **Check for conflicts**
   - Look up `.claude/commands/` for any existing skill with the same name
   - If conflict found and `--force` not set, prompt user to confirm overwrite or abort
   - If `--dry-run`, report what would happen and stop here

4. **Validate skill before import**
   - Run the same checks as `/skill/validate`
   - Warn about any missing metadata (description, usage examples, etc.)
   - Check for any dangerous or malformed content

5. **Copy skill to commands directory**
   - Determine target path: `.claude/commands/<skill-name>.md`
   - For directory imports, create a subdirectory: `.claude/commands/<skill-name>/`
   - Write the file(s) to the target location

6. **Update marketplace index (if applicable)**
   - If `.claude-plugin/marketplace.json` exists, check if this skill is already listed
   - If importing from a known marketplace URL, update the local registry entry

7. **Confirm import**
   - Display success message with the skill name and target path
   - Show a brief summary of what the skill does (first non-heading line)
   - Suggest running `/skill/info <skill-name>` to see full details

## Examples

```
# Import from a local file
/skill/import ~/Downloads/my-skill.md

# Import from a GitHub raw URL
/skill/import https://raw.githubusercontent.com/user/repo/main/skill.md

# Import with a custom name
/skill/import ./tools/analyzer.md --name code-analyzer

# Preview import without making changes
/skill/import ./new-skill.md --dry-run

# Overwrite an existing skill
/skill/import ./updated-skill.md --force
```

## Notes

- Imported skills are placed in `.claude/commands/` and immediately available
- Remote imports require network access; use `--dry-run` to verify the URL resolves correctly
- Skills imported from URLs will include a comment header noting the source URL and import date
- Use `/skill/validate <skill-name>` after import to ensure the skill works as expected
- To undo an import, use `/skill/remove <skill-name>`
