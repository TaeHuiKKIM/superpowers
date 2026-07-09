# Superpowers-Style Agent Operations

Use this when the user asks to use other models, agents, "superpowers", background workers, or maximum autonomous execution.

These patterns are adapted for Codex from practical agent setups: separate roles, explicit ownership, source-of-truth docs, scoped writes, and compact return reports.

## Role Split

Use disjoint roles instead of asking every agent to do everything:

| Role | Best For | Write Scope |
|---|---|---|
| Orchestrator/main agent | Critical path, architecture, final integration, deployment judgment | Owns integration files and final QA |
| Explorer | Read-only codebase inspection, risk lists, selector/file maps, test gaps | None |
| Implementer | Bounded feature implementation with judgment | Assigned modules only |
| Fast worker | Mechanical edits, docs, test scaffolds, formatting, simple verification | Assigned files only |
| Verifier | Independent screenshot/test review after changes | None or test artifacts only |

Do not spawn overlapping implementers against the same file set unless the user explicitly accepts merge risk.

## Delegation Prompt Template

Use a prompt like this:

```text
Repo: <path>.
You are not alone in the codebase. Other agents may be editing different files.
Do not revert or overwrite edits you did not make.

Task: <bounded task>.
Ownership: <files/modules you may edit>.
Do not touch: <files/modules owned by others>.
Required checks: <commands or inspection>.
Return: changed files, what you verified, risks, assumptions.
```

## Source Of Truth

Before implementation, identify the source of truth:

- Project docs or PRD.
- Existing README/roadmap.
- In-repo task files.
- Tests that encode expected behavior.
- Store/deployment docs for release constraints.

If docs and code disagree, inspect runtime behavior and update the stale doc or call out the mismatch.

## Checkpoint Protocol

For long autonomous work, keep checkpoints:

- Initial dependency map.
- Patch checkpoints after each coherent pass.
- Verification checkpoint with exact commands and screenshot paths.
- Release checkpoint after push/deploy.

When interrupted, resume from the latest checkpoint, not from memory.

## Safety Guardrails

- Do not delete, reset, or checkout away user work unless explicitly asked.
- Keep workers inside their assigned repo and write scope.
- Ask or escalate only for operations that truly need network, external writes, deployment, or GitHub repo creation.
- Never claim deployment or production verification from local tests alone.
- When using generated images, record prompts and final asset paths so future updates can reproduce the art direction.

## Return Contract

Each agent or work pass should return:

- `changed`: files changed or "none".
- `verified`: commands, screenshots, or manual checks.
- `risks`: what may still break.
- `next`: concrete follow-up if any.

The main agent must synthesize these returns into one final user-facing summary.

## Recommended Additions Beyond Basic Superpowers

- Add dependency maps for visual assets, metadata, safe-mode routes, and store screenshots.
- Add computed-layout Playwright assertions for bugs that escaped visual review.
- Keep a dated update log when UI/game feel changes are meaningful.
- Use image generation for weak visual fantasy, then test the asset in the running app.
- Production-test the exact reported bug after deploy.
- When a user wants the same agent structure reused in future repos, apply the Codex setup map in `codex-setup.md`: repo-local `AGENTS.md`, focused `.agents/skills`, project `.codex/config.toml`, optional hooks/rules, docs, and README sync.
