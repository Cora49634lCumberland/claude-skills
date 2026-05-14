# List Skills

List all installed skills or search for available skills in the marketplace.

## Usage

```
/skill/list [--installed] [--available] [--search <query>]
```

## Arguments

- `--installed` (optional): Show only installed skills (default behavior)
- `--available` (optional): Show skills available in the marketplace
- `--search <query>` (optional): Search for skills by name or description

## Steps

1. **Determine mode**: Check which flags were provided to determine what to display

2. **If `--installed` or no flags**:
   - Read `.claude-plugin/marketplace.json`
   - Filter for skills where `installed: true`
   - Display skill name, version, description, and author
   - Show total count of installed skills

3. **If `--available`**:
   - Read `.claude-plugin/marketplace.json`
   - List all skills with their installation status
   - Show skill name, version, description, author, and whether installed
   - Group by category if categories are present

4. **If `--search <query>`**:
   - Read `.claude-plugin/marketplace.json`
   - Search skill names and descriptions for the query (case-insensitive)
   - Display matching skills with installation status
   - Highlight matched terms if possible

5. **Output format**:
   ```
   Installed Skills (3):
   ─────────────────────────────────────
   ✓ skill-name         v1.0.0   Author Name
     Short description of what this skill does

   ✓ another-skill      v2.1.0   Author Name
     Short description of what this skill does
   ```

6. **If no skills found**: Display a helpful message suggesting `/skill/add` to install skills
