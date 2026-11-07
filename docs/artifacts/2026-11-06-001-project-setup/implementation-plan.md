# Implementation Plan: project setup toolchain

**Goal**: give the Blind King web app a working TypeScript toolchain so that every command in `.claude/project-commands.json` is real, as `/setup-project` requires.

**Decision source**: owner chose "web app only" on 2026-11-06. See `.claude/CLAUDE.local.md`.

**Files in scope**: `package.json`, `package-lock.json`, `tsconfig.json`, `eslint.config.js`, `vitest.config.ts`, `.gitignore`, `src/main.ts` (empty module, so the compiler has an input), `tests/*/.gitkeep`, `.claude/project-commands.json`.

**Non-goals**: no product code, no UI framework, no browser E2E tool, no licence file.

**Risks**: no real tests exist yet, so test commands pass via `--passWithNoTests`. Prettier is not installed, yet the resolver supplies a `format_check` default.

**Rollback**: nothing is committed; delete the files above.
