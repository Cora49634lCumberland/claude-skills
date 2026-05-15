# Skill Dependencies Command

Analyze and manage dependencies for installed Claude skills.

## Usage

```
/skill/deps [skill-name] [--check | --install | --list | --tree]
```

## Description

This command helps you understand and manage the dependencies required by Claude skills. Skills may depend on external tools, APIs, or other skills to function properly.

## Arguments

- `skill-name` (optional): Target a specific skill. If omitted, analyzes all installed skills.

## Flags

- `--check`: Verify all dependencies are satisfied without installing anything
- `--install`: Attempt to install missing dependencies automatically
- `--list`: Show flat list of all dependencies
- `--tree`: Show dependency tree (default)

## Steps

1. **Load skill manifest** — Read the skill's `manifest.json` or metadata block to extract declared dependencies
2. **Resolve dependency graph** — Build a dependency tree, detecting circular dependencies
3. **Check availability** — For each dependency, verify it is installed or accessible:
   - Other skills: check `.claude-plugin/marketplace.json` and local installs
   - CLI tools: check `$PATH`
   - Python packages: check `pip list` or `importlib`
   - APIs: validate required env vars are set (e.g., `OPENAI_API_KEY`)
4. **Report status** — Display each dependency with status:
   - ✅ Satisfied
   - ⚠️  Optional / degraded
   - ❌ Missing / broken
5. **Suggest fixes** — For missing dependencies, provide actionable install commands or links

## Example Output

```
Dependencies for skill: code-review

├── python >= 3.9          ✅ (3.11.2)
├── skill: git-tools       ✅ installed
├── skill: diff-viewer     ⚠️  optional, not installed
│   └── Install: /skill/add diff-viewer
├── GITHUB_TOKEN (env)     ✅ set
└── pylint >= 2.0          ❌ missing
    └── Install: pip install pylint

Summary: 3 satisfied, 1 optional, 1 missing
Run with --install to attempt automatic installation.
```

## Notes

- Skills should declare dependencies in their metadata under a `dependencies` key
- Use `--check` in CI pipelines to validate environment readiness before running skills
- Circular dependency detection will abort with an error listing the cycle path
- The `--install` flag only handles pip packages and skill dependencies automatically; system tools must be installed manually
