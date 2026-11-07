# Project Agent Routing Map

**Project**: blind-king
**Last updated**: 2026-11-06
**Owner**: Nelson Sousa

---

**What this file is**: this project's *deviations* from the standard agent chains in
`.claude/skills/skill-router/SKILL.md`.

---

## Project Shape

**Stack**: TypeScript web app (Vite, Vitest), see `.claude/CLAUDE.local.md`
**Has UI**: yes: minimal playback controls, plus voice as the main interface
**Has data pipelines**: possibly: importing and filtering Lichess puzzles, depending on whether puzzles ship with the app or come from the API
**Publishes content**: no
**Risk profile**: no auth, payments or PII planned. The main risks are CarPlay eligibility, spoken move recognition accuracy, and safe hands-free use while driving.

---

## Standing Routing Overrides

*(Empty: no deviations yet.)*

---

## Custom Chains

None yet.

---

## Mandatory Agents

Beyond the global non-skippable set (`requirements-analyst`, `verifier`, `change-reviewer`):

- `user-journey-evaluator`: for any change to the spoken or in-car flow, because this is used while driving

## Permitted Skips

None yet.

---

## Parallelisation

None yet.

---

## Review

**Reviewed on**: 2026-11-06 at project setup
**Next review**: when the stack is chosen
