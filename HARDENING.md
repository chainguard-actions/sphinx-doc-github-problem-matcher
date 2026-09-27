<!-- markdownlint-disable -->

# Hardening Report: sphinx-doc--github-problem-matcher/v1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **sphinx-doc--github-problem-matcher/v1.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression `${{ github.action_path }}` is interpolated directly inside a `run:` shell command string on line 11 of action.yml. Even though `github.action_path` is typically set by GitHub, any `${{ ... }}` expression inside a `run:` block undergoes YAML template substitution before the shell ever sees it, making it a script-injection risk. The offending line is: `run: echo '::add-matcher::${{ github.action_path }}/sphinx_matcher.json'`. The safe fix is to use the pre-set environment variable `$GITHUB_ACTION_PATH` instead: `run: echo "::add-matcher::${GITHUB_ACTION_PATH}/sphinx_matcher.json"`.

Locations:

- `action.yml:11`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script-injection in action.yml line 11: replaced `${{ github.action_path }}` with the pre-set environment variable `$GITHUB_ACTION_PATH`. Changed single quotes to double quotes so the shell variable expands correctly. The `GITHUB_ACTION_PATH` environment variable is set by GitHub Actions and is the safe, idiomatic way to reference the action's path without using a template expression inside a run: block.

