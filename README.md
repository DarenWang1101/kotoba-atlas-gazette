# The Kotoba Gazette

A separate newspaper-style edition of Kotoba Atlas, forked from Fieldnotes on 7 October 2026. Original newspaper masthead and composition; no affiliation with a newspaper publisher.

All original atlas interactions and audited data retained. Design overrides live in dist/gazette.css; structural newspaper header in dist/index.html. Static deployment uses dist.

Validation: node tests/ui-smoke.cjs. Browser visual QA was unavailable.

## GitHub Pages

`.github/workflows/pages.yml` validates the atlas, packages only `dist/`, and deploys through GitHub Actions on pushes to `main` or manual dispatch. Enable **Settings → Pages → Source: GitHub Actions** for the target repository. `scripts/prepare-pages.cjs` sets the deployed social URL from GitHub's Pages metadata; relative asset URLs support a project subpath. The Sites publication is unaffected.
