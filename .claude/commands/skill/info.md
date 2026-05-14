# Skill Info Command

Display detailed information about a specific skill from the claude-skills marketplace.

## Usage

```
/skill/info <skill-name>
```

## Arguments

- `skill-name` (required): The name or ID of the skill to inspect

## Description

This command retrieves and displays comprehensive details about a specific skill, including its description, version, author, dependencies, configuration options, and installation status.

## Steps

1. Parse the `skill-name` argument from the command input
2. Load the marketplace index from `.claude-plugin/marketplace.json`
3. Search for the skill by name or ID (case-insensitive)
4. If not found, suggest similar skills using fuzzy matching
5. Display the full skill details in a structured format
6. Check if the skill is currently installed by inspecting `.claude-plugin/` directory
7. Show installation status and any available updates

## Output Format

Display the following information when a skill is found:

```
╔══════════════════════════════════════════╗
║  Skill: <name>                           ║
╚══════════════════════════════════════════╝

ID:           <skill-id>
Version:      <version>
Author:       <author>
Status:       Installed / Not Installed
Update:       Up to date / Update available (<new-version>)

Description:
  <full description>

Tags: <tag1>, <tag2>, ...

Dependencies:
  - <dep1>
  - <dep2>

Configuration:
  <config-key>: <type> - <description>

Homepage:     <url>
Repository:   <repo-url>
License:      <license>

Installation:
  /skill/add <skill-id>
```

## Error Handling

- If no argument is provided, show usage instructions
- If skill is not found, display: `Skill '<name>' not found in marketplace`
- Suggest up to 3 similar skill names using partial string matching
- If marketplace.json is missing or malformed, show an appropriate error

## Examples

```
/skill/info git-helper
/skill/info code-review
/skill/info "python-linter"
```
