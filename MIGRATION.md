# Migration / Deployment Notes (Stanford mirror)

This repository is pulled onto the Stanford server so the same content appears at `https://ai.stanford.edu/~orrzohar/`.

## How the Stanford copy is updated
1. On the Stanford server, pull the latest changes from GitHub (recommended: `git pull --ff-only`).
2. If the Stanford site is served directly from this repo, no additional build step is needed.
3. Verify that the Stanford URL loads as expected after the pull.

## Canonical / SEO setup (so Google indexes the Stanford page, not github.io)
The Stanford pages are the canonical, indexable copy. The GitHub Pages site is a redirect that
*passes its authority* to Stanford. Two things make this work and must stay consistent:

1. **Stanford build self-canonicalizes.** `_layouts/default.html` emits a per-page
   `rel="canonical"` and `og:url` built from `{{ site.url }}{{ site.baseurl }}{{ page.url }}`
   (i.e. `https://ai.stanford.edu/~orrzohar/...`). `url` and `baseurl` are set in `_config.yml`.
   Do **not** hardcode `orrzohar.github.io` here — that was the bug that caused Stanford to be
   de-indexed in favor of github.io.
2. **github.io redirects without `noindex`.** `.github/workflows/redirect.yml` generates a minimal
   `_site/index.html` with a `canonical` to the Stanford URL + an instant (`0;`) meta refresh to it.
   Google treats an instant meta refresh as a *permanent* redirect and consolidates signals onto
   Stanford. Do **not** re-add `noindex` — that tells Google to drop the page instead of passing
   its authority to Stanford.

Deploy order matters: deploy/pull the Stanford self-canonical change **first**, then let the
github.io redirect (no-noindex) deploy. Then request indexing in Google Search Console for both
`https://ai.stanford.edu/~orrzohar/` and `https://orrzohar.github.io/`.

## Visitor tracking (analytics)
Because `https://orrzohar.github.io/` is only a redirect with `noindex`, visitor tracking should be implemented on the **Stanford** site, not the GitHub Pages redirect.

Options:
- **Server logs** (best for privacy-friendly counts): use Stanford/Apache access logs to count requests.
- **Client-side analytics** (more detail): add an analytics script to the Stanford homepage, such as Google Analytics 4, Matomo, or Plausible. Follow Stanford/department policy on tracking before enabling.

If adding analytics, do it on the Stanford-hosted pages, not the GitHub Pages redirect.

### Google Analytics 4 setup (already wired, just add an ID)
The site layout already includes `_includes/google_analytics.html`, which will load GA4 **only when a Measurement ID is configured**.

1. Create a GA4 property and copy the Measurement ID (format: `G-XXXXXXXXXX`).
2. Set it in `_data/main_info.yaml`:
   ```yaml
   google_analytics_measurement_id: "G-XXXXXXXXXX"
   ```
3. Commit and push, then pull the repo on the Stanford server so the change is deployed.

If the Measurement ID is left blank, the GA4 script will not load.
