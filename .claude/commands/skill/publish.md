# Skill Publish Command

Publish a local skill to the claude-skills marketplace.

## Usage

```
/skill/publish [skill-name] [options]
```

## Arguments

- `skill-name` - Name of the skill to publish (must exist in `.claude/commands/`)

## Options

- `--dry-run` - Validate and preview without actually publishing
- `--update` - Update an existing skill instead of creating new
- `--category <category>` - Specify marketplace category
- `--tags <tag1,tag2>` - Comma-separated list of tags

## Steps

1. **Validate skill file**
   - Check that `skill-name.md` exists in `.claude/commands/`
   - Verify the file has required sections: title, usage, description
   - Ensure the skill has at least one example

2. **Extract metadata**
   - Parse the skill markdown to extract:
     - Name and description from the title/header
     - Usage patterns from the `## Usage` section
     - Examples from the `## Examples` section
     - Any existing tags or categories

3. **Validate marketplace entry**
   - Check `.claude-plugin/marketplace.json` for duplicate entries
   - If `--update` flag is set, find and prepare to overwrite existing entry
   - If not `--update` and skill exists, prompt user to use `--update` instead

4. **Prepare marketplace entry**
   - Generate a unique skill ID (kebab-case from skill name)
   - Build the JSON entry:
     ```json
     {
       "id": "<skill-id>",
       "name": "<Skill Name>",
       "description": "<extracted description>",
       "category": "<category>",
       "tags": ["<tag1>", "<tag2>"],
       "command": "/<skill-name>",
       "version": "1.0.0",
       "author": "<git config user.name>",
       "publishedAt": "<ISO timestamp>"
     }
     ```

5. **Preview (dry-run)**
   - If `--dry-run` is set, display the entry that would be added
   - Show validation results and exit without modifying files

6. **Update marketplace.json**
   - Read current `.claude-plugin/marketplace.json`
   - Append or update the skill entry
   - Write back with proper formatting (2-space indent)

7. **Confirm publication**
   - Display success message with skill ID
   - Remind user to commit and push changes
   - Suggest running `/git/pr` to open a pull request

## Examples

```
/skill/publish my-custom-skill
/skill/publish my-custom-skill --dry-run
/skill/publish my-custom-skill --update --tags automation,productivity
/skill/publish my-custom-skill --category "Developer Tools"
```

## Notes

- Skills must follow the standard markdown template to be published
- The `--update` flag increments the patch version automatically
- Use `/skill/info` to verify the skill was published correctly
- Published skills become available via `/skill/search` after committing
