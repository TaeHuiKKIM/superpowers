# Agentic Ship Workflow

Use this for broad requests such as "make it production ready", "use agents", "study the repo first", or "fix everything thoroughly".

## Intake

Capture these before editing:

- User-visible goal and exact complaints.
- Current branch, dirty files, deploy target, and whether push/deploy is expected.
- Existing scripts: package scripts, test commands, screenshot commands, asset generators, deployment commands.
- Prior docs: README, docs, qa notes, launch kits, store submission packs, dated update logs.

## Study Pass

Read in this order:

1. README and product/design docs.
2. Build/test/deploy config.
3. Runtime entry points.
4. Source modules directly involved in the request.
5. Existing tests and QA scripts.
6. Existing screenshots or visual evidence.

Use `rg` for search and parallel file reads where possible.

## Dependency Map

Before edits, list what may need to change together:

- Runtime code.
- CSS/layout/theme.
- Generated/static assets.
- Tests and QA scripts.
- Docs, README, launch notes, screenshot READMEs.
- Metadata: OpenGraph, manifests, icons, service-worker cache lists.
- Deployment and production verification.

If this map reveals recurring drift risk, create or update a repo doc such as `docs/UI_DEPENDENCY_MAP.md`.

## Agent Pattern

Use this pattern when the user asked to use agents:

- Main agent: critical-path implementation, integration, final QA.
- Explorer 1: inspect risk areas and selectors/files, no edits.
- Worker 1: docs-only or test-only disjoint write set.
- Optional verification agent: run/read screenshots or review a specific finished surface.

Do not assign the next blocking edit to a subagent if the main agent can do it faster.

## Implementation Loop

1. Make the smallest coherent patch.
2. Run targeted checks.
3. Inspect output, screenshots, or computed layout metrics.
4. Add regression coverage for the exact bug that escaped.
5. Repeat until the user-visible issue and related surfaces are handled.

When a bug is visual, do not rely on "looks likely". Verify computed boxes, overlap, visibility, font overflow, safe-area position, and screenshots.

## Regression Guard Examples

- Bottom nav: assert computed `position: fixed`, viewport bottom gap, width, visible tab count, and safe-mode tab count.
- Character asset: assert topbar badge source, main avatar accessory state, image load, and no stale tier file.
- Store/social preview: assert meta tags, image dimensions, manifest references, and production HTTP 200.
- Tutorial/help: assert modal fits mobile and desktop, arrows stable, text does not overflow, safe-mode copy is removed.
- Generated image: assert file exists in repo, dimensions, alpha if needed, code references, service-worker cache entry when applicable.

## Final Pass

Before final response:

- Check git diff.
- Run relevant tests.
- Inspect any screenshots or browser surfaces that matter.
- Confirm docs and dependency map are updated.
- Confirm deploy status if deployment was requested.
- Summarize honestly, including anything not run.
