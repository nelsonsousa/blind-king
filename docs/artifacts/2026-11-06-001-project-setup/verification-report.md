---
basis_of:
  implementation-plan.md: cbfe303ae2326856c133d98428cbdebfd9254836250b44692652f137ea820bb0
  test-plan.md: 84cd5253f0d885a6d2ad9208b5a4f77ebe3e3df1b4504529b6d8ff18ac93d7c9
---

# Verification Report: project setup toolchain

Attempt: 1 of 3

## Overall status

Overall status: PARTIAL

Confidence: medium for the toolchain wiring (every command ran and exited as expected, including deliberate negative probes). Low for anything about product quality: there is no product code and no product test.

## Commands source

Commands come from `.claude/project-commands.json`, which is declared (written by `/setup-project`), not inferred. `python3 ~/.claude/hooks/project_commands.py` resolves the same set plus a default `format_check` (`npx prettier --check .`) that the declared file does not contain and that cannot run here (see Unverified).

No `checklist-results.md` exists; the task brief says it is not required for this setup task, so every check below was run directly.

Environment notes: the filesystem is slow (`npx eslint .` took 1m59s wall time, mostly I/O wait; vitest runs 9 to 12 s each). `git status` shows every file as untracked (no commit contains the scaffold), which matters for patch coverage below.

## Pre-commit re-run (2026-11-07)

Re-run requested before commit. Changes since the first run: `README.md`, `LICENSE`, `.claude/CLAUDE.local.md`, and the `license` field in `package.json`. Commands are the same declared set from `.claude/project-commands.json`. The mutation probes below were not repeated (they passed in the first run and their inputs are unchanged). This is the same attempt (no failure, no new hypothesis), so the attempt counter stays at 1 of 3.

```
$ npx eslint .
(no output; wall time 1m33s)
exit=0

$ npx tsc --noEmit
(no output; wall time 10s)
exit=0

$ npx vitest run tests/unit --passWithNoTests
 RUN  v4.1.12 /workspace/code/blind-king
No test files found, exiting with code 0
filter: tests/unit
exit=0

$ npx vitest run tests/integration --passWithNoTests
No test files found, exiting with code 0
filter: tests/integration
exit=0

$ npx vitest run tests/regression --passWithNoTests
No test files found, exiting with code 0
filter: tests/regression
exit=0

$ npx vitest run tests/e2e --passWithNoTests
No test files found, exiting with code 0
filter: tests/e2e
exit=0

$ npx vitest run --coverage --passWithNoTests
      Coverage enabled with v8
No test files found, exiting with code 0
Statements   : 100% ( 0/0 )
Branches     : 100% ( 0/0 )
Functions    : 100% ( 0/0 )
Lines        : 100% ( 0/0 )
exit=0
(coverage/cobertura-coverage.xml mtime moved from 2026-11-06 22:26:53 to 2026-11-07 11:49:05, size 670 bytes both times)

$ .venv/bin/diff-cover coverage/cobertura-coverage.xml --compare-branch=main
Diff: main...HEAD, staged and unstaged changes
No lines with coverage information in this diff.
exit=0

$ npx eslint src/main.ts
exit=0
```

Result: criteria 1 to 5 and the lint_file half of 7 reproduce the first run exactly. Criteria 6 and the red halves of 7 were not re-run (see above), so their evidence is the first run's. Nothing in the changed files (docs, licence text, one package.json metadata field) is executable, and lint and typecheck still pass, so the `license` edit did not break `package.json` parsing for eslint, tsc or vitest. Nothing in this re-run changes the coverage-gate or Unverified findings below: patch coverage is still 0 of 0 lines and still not measurable, and the other items stand.

## Results by criterion

| # | Criterion | Result | Evidence |
|---|---|---|---|
| 1 | Lint passes on the scaffold | PASS | `npx eslint .` exit 0, no output |
| 2 | Typecheck passes | PASS | `npx tsc --noEmit` exit 0, no output |
| 3 | Each test level runs | PASS (vacuous) | all four exit 0, each reports "No test files found" |
| 4 | Coverage writes a report | PASS (vacuous) | exit 0, `coverage/cobertura-coverage.xml` mtime moved from 22:12:04 to 22:26:53, so it was rewritten by this run |
| 5 | Patch coverage runs | PASS (vacuous) | `diff-cover` exit 0, "No lines with coverage information in this diff." |
| 6 | Checks can go red | PASS | type error, lint error and failing test each gave non-zero exit (see Mutation probes) |
| 7 | Single-file commands work | PASS | `lint_file` on `src/main.ts` exit 0 and on a lint-error probe exit 1; `test_one` on a probe test exit 0 and on a red probe exit 1 |

## Commands run and output

```
$ npx eslint .
(no output)
exit=0

$ npx tsc --noEmit
(no output)
exit=0

$ npx vitest run tests/unit --passWithNoTests
 RUN  v4.1.12 /workspace/code/blind-king
No test files found, exiting with code 0
filter: tests/unit
exit=0

$ npx vitest run tests/integration --passWithNoTests
No test files found, exiting with code 0
filter: tests/integration
exit=0

$ npx vitest run tests/regression --passWithNoTests
No test files found, exiting with code 0
filter: tests/regression
exit=0

$ npx vitest run tests/e2e --passWithNoTests
No test files found, exiting with code 0
filter: tests/e2e
exit=0

$ npx vitest run --coverage --passWithNoTests
      Coverage enabled with v8
No test files found, exiting with code 0
 % Coverage report from v8
File      | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s
Statements   : 100% ( 0/0 )
Branches     : 100% ( 0/0 )
Functions    : 100% ( 0/0 )
Lines        : 100% ( 0/0 )
exit=0

$ .venv/bin/diff-cover coverage/cobertura-coverage.xml --compare-branch=main
Diff: main...HEAD, staged and unstaged changes
No lines with coverage information in this diff.
exit=0

$ npx eslint src/main.ts
exit=0
```

## Coverage gates

| Gate | Threshold | Measured | Verdict |
|---|---|---|---|
| Patch coverage | >= 90% | not measurable: 0 of 0 lines. `src/main.ts` is `export {};` (no executable lines), and the scaffold is entirely untracked, so `diff-cover` sees no diff to grade | Cannot be measured here. Not a pass. |
| Project coverage vs baseline | must not decrease | 0/0 statements, branches, functions, lines (reported as 100%, which is vacuous). Baseline is the same empty report. | Cannot be meaningfully compared |
| Branch coverage, changed lines in critical paths | 100% | No critical paths declared in `CLAUDE.local.md`; 0 branches exist | Not applicable |
| Acceptance criteria with a passing test | 100% | No product acceptance criteria exist. The 7 setup criteria are checked by command, not by test files. | Not applicable |

What coverage can show: that the vitest v8 provider is installed and configured, that `coverage/cobertura-coverage.xml` is written in the format `diff-cover` reads, and that `diff-cover` runs and exits 0 against it. What it cannot show: anything about whether future code will be covered. The "100%" figure is an empty set, so it carries no information. It also has not been shown that `include: ["src/**/*.ts"]` makes uncovered source files appear with a real percentage, since the only source file has no statements. Coverage floors must be re-measured when the first product code lands.

The coverage baseline was not written to `coverage-baseline.json`: the number is 0/0, and writing a file would add a "baseline" with no content (and the brief said to change nothing else).

## Mutation probes (criterion 6 and 7, deliberate negative checks)

Throwaway files were created in `src/` and `tests/unit/`, run, and removed. These results are intended; they are not failures of the scaffold.

```
$ npx tsc --noEmit        (with src/zz-probe-type.ts: export const n: number = "not a number";)
src/zz-probe-type.ts(1,14): error TS2322: Type 'string' is not assignable to type 'number'.
exit=2

$ npx eslint src/zz-probe-lint.ts        (const unused = 1;)
  1:7  error  'unused' is assigned a value but never used  @typescript-eslint/no-unused-vars
1 problem (1 error, 0 warnings)
exit=1

$ npx vitest run tests/unit --passWithNoTests        (one red probe and one green probe present)
 FAIL  tests/unit/zz-probe-fail.test.ts > probe fails
AssertionError: expected 1 to be 2 // Object.is equality
 Test Files  1 failed | 1 passed (2)
      Tests  1 failed | 1 passed (2)
exit=1

$ npx vitest run tests/unit/zz-probe-pass.test.ts        (test_one, green)
 Test Files  1 passed (1)
      Tests  1 passed (1)
exit=0

$ npx vitest run tests/unit/zz-probe-fail.test.ts        (test_one, red)
      Tests  1 failed (1)
exit=1
```

Cleanup:

```
$ rm -f tests/unit/zz-probe-fail.test.ts tests/unit/zz-probe-pass.test.ts src/zz-probe-type.ts src/zz-probe-lint.ts && ls -A src tests/unit
src:
main.ts

tests/unit:
.gitkeep
```

The red probe also confirms `--passWithNoTests` does not mask a real red test, and that the `tests/unit` filter is honoured.

## Failures and root causes

No failures in the setup criteria. The one non-zero exit not caused by a deliberate probe is `npx --no-install prettier --check .` (environment: prettier is not installed, so npx declined to fetch it). It is a known gap listed in the test plan, not a defect in the scaffold. See Unverified.

## Side effects of this run

- `coverage/cobertura-coverage.xml` was regenerated (same size, 670 bytes, new timestamp). The directory is gitignored.
- No other file was changed. `git status --short` after cleanup lists the same untracked set as before.

## Unverified

| Area | Reason | Mitigation |
|---|---|---|
| Patch coverage (>= 90%) | Not measurable here: no executable product lines, and every file is untracked so `diff-cover` has no diff to read | Re-measure on the first commit that adds product code; commit the scaffold first so `main...HEAD` is meaningful |
| Project coverage non-regression | Baseline and current are both an empty 0/0 set | Record a real `coverage-baseline.json` once product code exists |
| Coverage `include` pattern on real source | The only source file, `src/main.ts`, has no statements, so uncovered files appearing as 0% was never observed | Check with the first real module |
| Product behaviour, all test levels | No product tests exist; unit, integration, regression and e2e levels pass only via `--passWithNoTests` | First feature must bring tests written from its acceptance criteria |
| E2E | `tests/e2e` is a Vitest placeholder; no browser test tool chosen | Choose and wire one before any user journey is built |
| `format_check` (prettier) | Not installed (`npx --no-install prettier --check .` exits 1, package missing); resolver supplies it as a default but it is absent from `project-commands.json` | Install prettier and declare the command, or drop the default; also a latent trap for any agent that runs the resolved map blindly |
| Fresh-clone setup | The `.venv` / `diff-cover` install steps in `CLAUDE.local.md` were not re-run from scratch | Run `python3 -m venv .venv && .venv/bin/pip install diff-cover` on a clean clone |
| Dependency licences | Out of scope; `CLAUDE.local.md` records dev-only `minimatch` (BlueOak-1.0.0) as outside the allowed list pending owner answer | Owner decision |
| Review and release phases | Not run (change-reviewer, release-steward) | Next in chain |
