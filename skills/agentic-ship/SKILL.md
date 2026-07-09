---
name: agentic-ship
description: End-to-end agentic development workflow for high-polish product updates and Codex agent setup. Use when the user asks Codex to work autonomously, use other agents/models, apply superpowers, port Claude Code-style agent workflows, create AGENTS.md/.codex/.agents structures, deeply study docs/code, improve UI/game feel, create/update docs, keep dated update logs, cross-validate with tests/screenshots, update README/store/social/deployment assets, push/deploy, or ship production-ready changes with careful regression coverage.
---

# Agentic Ship

Use this skill to turn a broad product request into a shipped, verified update instead of a single patch.

## Core Loop

1. Restate the outcome in concrete terms: user-visible behavior, affected surfaces, and release target.
2. Read the repo first: docs, README, package scripts, key source files, existing QA, deployment docs, and any prior dated update notes.
3. Build a dependency map before editing: runtime files, generated assets, tests, docs, metadata, screenshots, service-worker/cache entries, and deployment targets.
4. Delegate sidecar work when the user asked for agents or parallel model use. Keep immediate blocking work local; use subagents for independent codebase inspection, docs-only updates, or verification.
5. Implement in small coherent passes. Do not declare completion until visual checks, automated checks, documentation, and deployment status match the user request.
6. Cross-validate from at least two angles: automated tests plus browser/screenshot/layout inspection for UI work, or unit/integration plus source-review for logic work.
7. Update future-maintenance docs whenever a change has cross-file coupling. Include known drift patterns and which tests/screenshots catch them.
8. Record what changed by date when the repo has dated update docs, launch kits, screenshots, or README history.
9. For release work, verify production after deploy, not only local state.

## Reference Routing

- For any large autonomous coding task, read `references/workflow.md`.
- For UI, frontend, game-feel, sprites, mockups, generated images, tutorial, or visual QA, read `references/ui-game-qa.md`.
- For README, docs, dated logs, social preview, store/Toss/Play/Vercel, git push, or deployment, read `references/docs-release.md`.
- For explicit "use agents/models/superpowers" requests, read `references/superpowers.md`.
- For Claude Code-style setup, Codex repo conventions, `AGENTS.md`, `.agents/skills`, `.codex/config.toml`, hooks, rules, MCP, custom agents, or secret-safe agent scaffolding, read `references/codex-setup.md`.

## Delegation Rules

- Spawn agents only when the user explicitly asked for other agents/models, delegation, or parallel agent work.
- Split work by ownership. Example: one explorer inspects UI risks, one worker updates docs, main agent edits app code and tests.
- Tell worker agents they are not alone in the codebase and must not revert others' edits.
- Prefer explorers for read-only questions and workers for disjoint write sets.
- Integrate and review subagent output; do not blindly trust it.
- Require each subagent to return changed files, checks run, remaining risks, and any assumptions.

## Image Asset Rule

If UI still feels weak after code-native polish, create real raster assets rather than overloading CSS. Use the `imagegen` skill for generated bitmaps, then move selected assets into the repo, wire them into code, and test every surface that references them.

## Completion Standard

Final status must say:

- What changed.
- What was verified locally and, if relevant, in production.
- What docs/screenshots/metadata were updated.
- Any deployment or approval step that could not be completed.
