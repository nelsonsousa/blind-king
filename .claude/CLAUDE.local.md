# Project-Specific Claude Configuration

## Project Identity

- **Project name**: blind-king (title: Blind King)
- **Repo**: git@github.com:nelsonsousa/blind-king.git (public)
- **Trello board**: https://trello.com/b/AmMIt59x/chess
- **Git model**: 0 (Simple): one clone, feature branches off `main`, PRs to `main`
- **Brief**: `docs/PROJECT-BRIEF.md` (the owner's words, verbatim)

## Claude's Role in This Project

[To be filled by human: describe Claude's scope and decision authority]

## Project-Specific Permissions

The container proxy currently blocks `lichess.org`, `database.lichess.org` and
`developer.apple.com` (checked 2026-11-06). Anything needing them needs an entry in the host
`docker/allowlist.txt`.

## Workflow Overrides

None.

## Tech Stack & Conventions

**Web app only** (owner's decision, 2026-11-06). The owner has no Apple signing account. A
later move to native Swift stays possible and is not in scope now.

- TypeScript, built with Vite. No UI framework chosen yet.
- Vitest (with v8 coverage), ESLint with typescript-eslint.
- Patch coverage uses `diff-cover` from a local gitignored `.venv`. On a fresh clone run
  `python3 -m venv .venv && .venv/bin/pip install diff-cover`.
- `tests/e2e` is a Vitest placeholder until a browser test tool is chosen.
- Commands live in `.claude/project-commands.json`.

## Licensing

This project is MIT licensed (`LICENSE`). Licence tiers, as set by the owner:

- **Available**: MIT, Apache 2.0, BSD 2-Clause, BSD 3-Clause, ISC, Zlib, Boost Software License
  1.0, Unlicense, CC0 1.0, MPL 2.0, EPL 2.0, 0BSD, PostgreSQL, CC BY, CC BY-SA.
- **Requires specific approval, so avoid**: GPL v2, GPL v3, LGPL v2.1, LGPL v3, AGPL v3.
- **Not acceptable**: Creative Commons with NC or ND, Business Source License, SSPL, Elastic,
  Commons Clause, PolyForm, other source-available licenses.

The owner checks licence compatibility before moving forward with a dependency, so report a new
dependency's licence and get the owner's go-ahead before adding it. Dev-only `minimatch`
(BlueOak-1.0.0) is in none of the tiers and is for the owner to review.

## Known Issues or Constraints

- No CarPlay screen: a web app cannot appear on it, as far as Apple's documentation shows. Native
  CarPlay would need a paid developer account and an Apple-granted entitlement.
- This is used while driving: interaction must be hands-free and must not need the phone screen.
- Owner's idea, unverified: drive the app as a media session (Media Session API
  `nexttrack`/`previoustrack`) so steering wheel skip buttons work. Sources confirm these handlers
  on iOS Safari with an `<audio>` element, but nothing confirms car controls reach them, or that
  they work with `speechSynthesis`. Needs a test on a real iPhone in the car, and may mean
  narration has to be audio files rather than synthesized speech.
- The repo is public, so everything committed, including `docs/artifacts/`, is public.

## Critical Paths

None declared yet: no auth, payments or user-data store is planned. Revisit once the stack is
chosen (for example if accounts or progress sync are added). To be confirmed by human.

## Definition of Done

Assembled from the framework rules; those rules stay authoritative.

- All acceptance criteria met, each mapped to at least one passing test.
- Patch coverage at least 90%; project coverage does not decrease; 100% branch coverage on
  changed lines in critical paths. Where coverage cannot be measured, status is PARTIAL and says so.
- Bug fixes have a regression test seen failing before the fix.
- Phase order followed; at most three implement and verify cycles, then escalate.
- Lint, typecheck and all test levels pass.
- Requirement premises tested against real data, or recorded as risks.
- Change review approved, rollback plan documented where applicable, changelog updated where
  applicable.

Project-specific additions: [To be confirmed by human]

## Communication Preferences

- **Interactive session**: Inline messages
- **Blockers**: Trello card in the Blocked lane, always, checked via `/remote-control`
- **Release communication**: [To be filled by human]

## Escalation Path

[To be filled by human: when to stop and ask for approval]
