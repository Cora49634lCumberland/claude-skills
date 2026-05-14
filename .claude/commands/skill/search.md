# Skill Search Command

Search for available skills in the marketplace by keyword, category, or tag.

## Usage

```
/skill/search <query>
```

## Arguments

- `$ARGUMENTS` - Search query (keyword, category name, or tag)

## Instructions

1. Read the marketplace data from `.claude-plugin/marketplace.json`

2. Parse the `$ARGUMENTS` to extract the search query
   - If no arguments provided, list all available categories and prompt user to refine

3. Search across the following fields in each skill entry:
   - `name` - skill name
   - `description` - skill description
   - `tags` - associated tags
   - `category` - skill category
   - `author` - skill author

4. Perform case-insensitive matching against the query

5. Rank results by relevance:
   - Exact name match → highest priority
   - Name contains query → high priority
   - Tag exact match → medium-high priority
   - Description contains query → medium priority
   - Author match → lower priority

6. Display results in a formatted table:

```
Search results for: "<query>"
Found <N> skill(s)

┌─────────────────────────────────────────────────────────────────┐
│ Name          │ Category     │ Description              │ Tags  │
├─────────────────────────────────────────────────────────────────┤
│ skill-name    │ category     │ Short description...     │ tag1  │
└─────────────────────────────────────────────────────────────────┘
```

7. After displaying results, show install hint:
   ```
   To install a skill, run: /skill/add <skill-name>
   ```

8. If no results found:
   - Show message: `No skills found matching "<query>"`
   - Suggest browsing categories: `Try searching by category: coding, writing, research, productivity`
   - Suggest checking installed skills: `To see installed skills, run: /skill/list`

## Examples

```
/skill/search python
/skill/search code review
/skill/search productivity
```
