# Migration / Deployment Notes (Stanford mirror)

This repository is pulled onto the Stanford server so the same content appears at `https://ai.stanford.edu/~orrzohar/`.

## How the Stanford copy is updated
1. On the Stanford server, pull the latest changes from GitHub (recommended: `git pull --ff-only`).
2. If the Stanford site is served directly from this repo, no additional build step is needed.
3. Verify that the Stanford URL loads as expected after the pull.

## Redirect behavior for `https://orrzohar.github.io/`
The GitHub Pages deployment is handled by `.github/workflows/redirect.yml`, which generates a minimal `_site/index.html` containing:
- `noindex,follow` meta tags (robots + googlebot)
- canonical link to the Stanford URL
- immediate meta refresh to the Stanford URL
- a fallback body link

If any of these requirements change, update the heredoc in the workflow and re-deploy.

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
