# Test Plan: project setup toolchain

This is tooling, not product behaviour, so the criteria are about the commands.

| # | Criterion | Check |
|---|---|---|
| 1 | Lint passes on the scaffold | `npx eslint .` exits 0 |
| 2 | Typecheck passes | `npx tsc --noEmit` exits 0 |
| 3 | Each test level runs | `npx vitest run tests/<level> --passWithNoTests` exits 0 for unit, integration, regression, e2e |
| 4 | Coverage writes a report | `npx vitest run --coverage --passWithNoTests` exits 0 and writes `coverage/cobertura-coverage.xml` |
| 5 | Patch coverage runs | `.venv/bin/diff-cover coverage/cobertura-coverage.xml --compare-branch=main` exits 0 |
| 6 | Checks can fail | A throwaway failing test, type error and lint error each give a non-zero exit. Remove them afterwards. |
| 7 | Single-file commands work | `lint_file` and `test_one` from `project-commands.json` run on a real file and exit as expected |

**Known gap**: there are no product tests, and `format_check` (prettier) is not installed.
