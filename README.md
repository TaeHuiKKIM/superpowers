# Superpowers

Codex plugin for high-polish, agentic product shipping.

It includes the `agentic-ship` skill, which turns broad requests like "use agents and ship it properly" into a repeatable workflow:

- study docs and code before editing
- split work across explorers, implementers, fast workers, and verifiers
- map runtime, assets, tests, docs, metadata, screenshots, and deployment dependencies
- use generated raster assets when CSS/SVG placeholders are not enough
- add regression tests for bugs that escaped review
- keep README, dated docs, store/social metadata, and deployment evidence current
- verify production after deployment

## Install Locally

This plugin is scaffolded for the default personal Codex marketplace at:

```text
~/.agents/plugins/marketplace.json
```

The plugin source lives at:

```text
~/plugins/superpowers
```

Use a new Codex thread after installing or updating so the skill metadata is reloaded.

## Main Skill

Invoke:

```text
Use $agentic-ship to plan, implement, cross-validate, document, and ship this update.
```

## Contents

```text
.codex-plugin/plugin.json
skills/agentic-ship/SKILL.md
skills/agentic-ship/references/
```

## License

MIT
