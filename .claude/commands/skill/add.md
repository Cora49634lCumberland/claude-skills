# Add Skill Command

Install a skill from the claude-skills marketplace into your project.

## Usage

```
/skill/add <skill-name>
```

## Arguments

- `skill-name`: The name of the skill to install (as listed in `.claude-plugin/marketplace.json`)

## What This Does

1. Looks up the skill in `.claude-plugin/marketplace.json`
2. Validates the skill exists and is compatible with your Claude version
3. Copies the skill's command files into `.claude/commands/`
4. Updates any skill registry or index if present
5. Confirms installation with a summary of what was added

## Steps

1. Read `.claude-plugin/marketplace.json` and find the entry matching `$ARGUMENTS`
2. If not found, list available skills and ask the user to pick one
3. Check if the skill is already installed by looking for its files in `.claude/commands/`
4. If already installed, ask whether to reinstall/update
5. Extract the skill's files and write them to `.claude/commands/<skill-category>/`
6. Print a confirmation message listing all installed files

## Example

```
/skill/add git-flow
```

Installs the `git-flow` skill, adding commands like:
- `.claude/commands/git/flow-start.md`
- `.claude/commands/git/flow-finish.md`
- `.claude/commands/git/flow-release.md`

## Notes

- Skills are scoped to the current project only
- To share skills across projects, copy them to your global `~/.claude/commands/` directory
- Use `/skill/list` to browse available skills before installing
