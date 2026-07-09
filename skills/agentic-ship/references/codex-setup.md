# Codex Setup And Agent Structure

Use this when the user asks to apply a reusable agent structure to a repo, port Claude Code-style tips into Codex, create durable repo instructions, or make "superpowers" available on future projects.

## Default Scope

Default to repo-local setup. Do not edit global `~/.codex`, global `~/.agents`, or every-project defaults unless the user explicitly asks for behavior across all repos.

Before writing files:

- Check `git status --short --branch`.
- Read existing `AGENTS.md`, `.codex/`, `.agents/`, `docs/`, `README.md`, and any Claude/Cursor/Gemini instruction files.
- Preserve user changes and existing instructions. Add narrowly scoped sections instead of replacing whole files.
- If the repo is a plugin or skill repo, update the skill/reference that owns the behavior instead of adding unrelated top-level files.

## Surface Map

Choose the smallest durable surface that matches the user's intent:

| Need | Codex Surface |
|---|---|
| One-off constraint for this run | User prompt or thread context |
| Repo conventions, test commands, docs rules, review expectations | `AGENTS.md` |
| Project Codex defaults, trusted hooks, MCP, feature flags, agents | `.codex/config.toml` |
| Reusable task workflow | `.agents/skills/<skill>/SKILL.md` |
| Shareable bundle of skills, apps, MCP, assets | Codex plugin |
| External live tools or private workspace data | MCP server or app connector |
| Mechanical lifecycle enforcement | `.codex/hooks.json` plus scripts |
| Command allow/deny policy outside sandbox | `.codex/rules/*.rules` |
| Parallel specialized work | `.codex/agents/*.toml` or built-in subagents |

## Claude Code To Codex Mapping

When importing agent tips from a Claude Code setup, translate by purpose:

| Claude-style item | Codex destination |
|---|---|
| `CLAUDE.md`, instruction files | `AGENTS.md` |
| `settings.json` or tool defaults | `config.toml` |
| Slash commands | Codex skills |
| Reusable commands/workflows | `.agents/skills` |
| Subagents | `.codex/agents/*.toml` |
| MCP configuration | `config.toml` `[mcp_servers.*]` |
| Hooks | `.codex/hooks.json` or `[hooks]` in config |
| Permission rules | `.codex/rules/*.rules` |
| Shared pack of all of the above | Codex plugin |

## Recommended Repo Structure

Create only what the repo needs:

```text
AGENTS.md
.agents/
  skills/
    <focused-skill>/
      SKILL.md
      references/
.codex/
  config.toml
  hooks.json
  hooks/
  rules/
  agents/
docs/
  codex-workflow.md
README.md
.gitignore
```

For simple repos, start with `AGENTS.md`, `docs/codex-workflow.md`, and `README.md`. Add hooks, rules, MCP, or custom agents only when they remove real repeated friction.

## AGENTS.md Baseline

Use this as a concise starting point when the repo lacks durable agent guidance:

```md
# AGENTS.md

## Repository Expectations

- Before development, create or update `docs/` with planning notes, implementation decisions, troubleshooting, or verification evidence when the change is meaningful.
- Keep `README.md` current whenever setup, behavior, commands, outputs, or workflow expectations change.
- Prefer existing project patterns over new abstractions.
- Run the most relevant build, lint, test, or manual verification before reporting completion.

## Security

- Never stage or commit `.env` files, API keys, tokens, credentials, private certificates, or secret-bearing logs.
- Do not include secrets even when explicitly asked to push all files.
- Keep secrets in local environment variables or ignored files.
- Before commits, inspect staged diffs for secret-looking values.

## Git Safety

- Do not reset, checkout away, or delete user work unless the user explicitly asks for that exact operation.
- Stage only files that belong to the requested change.
```

Keep project-specific commands below this baseline. Nested `AGENTS.md` files can override rules for subdirectories.

## Project Config Guidance

Use `.codex/config.toml` for durable project settings only after confirming the project is trusted. Avoid pinning model, sandbox, or approval settings unless the user requested those choices.

Reasonable project-local examples:

```toml
[agents]
max_threads = 6
max_depth = 1
```

```toml
[mcp_servers.context7]
command = "npx"
args = ["-y", "@upstash/context7-mcp"]
enabled = true
```

Do not put API keys directly in config. Use `env_vars` or environment variable names.

## Skill Guidance

Create repo skills when a workflow will be reused by future Codex threads. Keep each skill focused:

- `plan-and-doc`: planning, `docs/`, README sync.
- `secret-safe-commit`: pre-commit review and secret checks.
- `release-verify`: build/test/deploy verification.
- `frontend-qa`: browser screenshots, layout checks, visual regressions.

Prefer reference files for detailed checklists so `SKILL.md` stays short.

## Hook Guidance

Use hooks for deterministic checks, not judgment-heavy review. Good hook candidates:

- Block or warn on `.env` and credential filenames.
- Scan staged diffs for secret-looking values.
- Remind when `README.md` or `docs/` may need updates after broad changes.
- Enforce repo-specific command safety.

Hook scripts must be deterministic, fast, and safe to run from the repo root. After adding or changing hooks, tell the user they may need to trust them in Codex.

## Secret-Safe Commit Checklist

Before staging or pushing:

1. Run `git status --short`.
2. Confirm `.env`, `.env.*`, key files, credential files, and generated secret logs are ignored or unstaged.
3. Inspect `git diff --cached` before commit.
4. Search staged text for obvious secret patterns such as `api_key`, `secret`, `token`, `BEGIN PRIVATE KEY`, `sk-`, `ghp_`, `gho_`, and provider-specific key prefixes.
5. If a secret was staged, unstage it and rotate the credential if it may have been exposed.

## Documentation Sync Checklist

When adding this structure to a repo:

- Add or update `docs/codex-workflow.md` with the selected surfaces and how to invoke them.
- Update `README.md` with the new agent workflow, setup notes, and useful prompts.
- Record troubleshooting notes under `docs/` when setup required special handling.
- Avoid duplicate docs. If an existing doc owns the topic, update it in place.

## Final Report

When setup is complete, report:

- Files added or updated.
- Which surfaces are now active (`AGENTS.md`, skills, hooks, config, rules, agents, MCP).
- Any manual restart, plugin reinstall, hook trust, or app refresh needed.
- Verification performed and any intentionally skipped checks.
