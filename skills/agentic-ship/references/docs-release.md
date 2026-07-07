# Docs, Release, And Deployment Hygiene

Use this when the request touches README, docs, dated updates, social/store metadata, git push, Vercel, Toss, OneStore, Google Play, or production verification.

## Docs To Consider

Update existing repo conventions first. Common targets:

- `README.md`
- `docs/`
- `docs/update/`
- screenshot READMEs
- QA readiness docs
- store submission packs
- launch kits
- deployment/process docs
- dependency maps such as `docs/UI_DEPENDENCY_MAP.md`

Do not create duplicate docs if an existing doc clearly owns the topic.

## Dated Update Notes

When a repo keeps dated notes or screenshots:

- Add a dated entry for meaningful UI, asset, gameplay, or deployment changes.
- Include exact surfaces changed.
- Include test/screenshot evidence.
- Mention follow-up risks and related files.

Good entry shape:

```markdown
## 2026-07-07 - Mobile first-screen clarity

- Moved primary action buttons above market news.
- Made portfolio quick-sell the single sticky full-sell CTA.
- Added Playwright assertions for fixed bottom nav and scene object overlap.
- Verified: `npm run test:ui`, item visual matrix, production route smoke.
```

## README And Portfolio Updates

If the user wants portfolio/GitHub presentation updated:

- Update feature bullets to match actual shipped behavior.
- Replace stale screenshots or thumbnails.
- Mention build-time or launch context only if accurate.
- Keep claims verifiable by docs or tests.

## Social And Store Metadata

When changing visual identity or thumbnails, check:

- `index.html` OpenGraph and Twitter tags.
- `assets/og-thumbnail.png` dimensions and freshness.
- App icons and manifests.
- Splash/key visual assets.
- Service worker cache list/version if used.
- Store/Toss/Play screenshots and submission docs.

Verify deployed URLs return HTTP 200 for key assets.

## Deployment Flow

Before deploy:

1. Confirm branch and git diff.
2. Run unit/build/static checks.
3. Run UI tests relevant to changed surfaces.
4. Run production-readiness scripts if available.
5. Capture screenshots if visual/store-facing changes occurred.

Deploy:

- Use the repo's documented deployment path.
- If Vercel is used, confirm the production alias points to the new deployment.
- Push the branch/main only when the user requested or prior workflow requires it.

After deploy:

1. Run production verification scripts.
2. Open key routes: `/`, safe/review routes, guide/privacy/terms/support if relevant.
3. Verify metadata and assets from production URLs.
4. Re-test the exact bug that triggered the work against production, not only local.

## Final Report

Report:

- Commit/branch/deployment if created.
- Key changed files.
- Tests run.
- Production URL and verification status.
- Known residual risk or manual store-review step still required.
