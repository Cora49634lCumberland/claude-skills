# Test Command

Run tests for the current project with intelligent test selection and reporting.

## Usage

```
/test [scope] [options]
```

## Arguments

- `scope` (optional): Specific test file, directory, or test name pattern to run
- `options` (optional): Additional flags like `--coverage`, `--watch`, `--verbose`

## Behavior

1. **Detect test framework** — Inspect `package.json`, `pyproject.toml`, `setup.cfg`, or `Makefile` to identify the test runner (pytest, jest, vitest, mocha, etc.)
2. **Determine scope** — If a scope is provided, run only matching tests. Otherwise run the full suite.
3. **Run tests** — Execute the appropriate test command with sensible defaults.
4. **Summarize results** — Report pass/fail counts, duration, and any failures with file + line references.
5. **Suggest fixes** — For each failing test, briefly describe the likely cause and suggest a fix.

## Examples

```bash
# Run all tests
/test

# Run tests in a specific file
/test tests/test_skills.py

# Run tests matching a pattern
/test skill_loader

# Run with coverage report
/test --coverage

# Watch mode for TDD
/test --watch
```

## Notes

- If no test framework is detected, ask the user which runner to use before proceeding.
- Always show the exact command being run before executing it.
- For coverage reports, open or print the summary inline — do not just say "coverage report generated".
- If tests pass, confirm with a brief success message and the total count.
- If tests fail, list each failure with its file path, line number, and error message.
