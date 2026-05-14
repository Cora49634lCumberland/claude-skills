# Code Review

Perform a thorough code review of the specified files or recent changes.

## Usage

```
/review [files or scope]
```

## Arguments

- `$FILES` - Files or directories to review (optional, defaults to staged/recent changes)

## What This Does

1. **Analyzes code quality** — checks for readability, maintainability, and style consistency
2. **Identifies bugs** — looks for logic errors, edge cases, and potential runtime issues
3. **Security review** — flags potential security vulnerabilities or unsafe patterns
4. **Performance** — highlights inefficient patterns or unnecessary complexity
5. **Test coverage** — notes areas that lack tests or have weak assertions
6. **Documentation** — checks for missing or outdated docstrings and comments

## Review Format

For each issue found, provide:
- **Severity**: `critical` | `major` | `minor` | `suggestion`
- **Location**: file and line number
- **Description**: what the issue is and why it matters
- **Recommendation**: concrete suggestion to fix or improve it

## Example Output

```
### Review Summary
- 0 critical issues
- 1 major issue
- 3 minor issues
- 2 suggestions

---

**[MAJOR]** `src/auth.py:42`
Password is logged in plain text during debug mode.
Recommendation: Mask sensitive fields before logging.

**[MINOR]** `src/utils.py:17`
Function `parse_date` has no docstring.
Recommendation: Add a docstring describing parameters and return type.
```

## Instructions

Review the files provided (or recent git diff if none specified): $FILES

Be constructive and specific. Prioritize issues that affect correctness and security over style.
