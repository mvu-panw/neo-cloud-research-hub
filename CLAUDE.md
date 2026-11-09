# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Static link-index site for researching three neo-cloud accounts: CoreWeave, Lambda AI, and Nebius. There is no build step, framework, test suite, or linter. What's committed is what ships.

## Run locally

```bash
python3 -m http.server 8080
```

Use a server rather than opening `index.html` from disk. The home page loads `news.md` with `fetch()`, and that fails under `file://`.

## Deploy

`.github/workflows/deploy.yml` uploads the whole repo root to GitHub Pages on every push to `main`. That makes every tracked file public, so keep scratch files and internal exports out of the repo.

## Structure

- **Pages.** Each vendor has 5 pages named `{coreweave|lambda|nebius}-{regions|network-security|compute|blogs|newsroom}.html`. Each page is a `.page-header` plus a `.resource-grid` of `<a class="resource-card">` links to vendor docs. External links use `target="_blank" rel="noopener noreferrer"`. Each grid ends with a commented-out card template.
- **`ai-district-braintrust.html`** is the one data page. It has stat cards and `.data-table` tables (inside a `.table-wrap` that scrolls sideways on mobile), with Disclosed/Reported/Estimate `.tag` labels and a numbered source list (`#sN` anchors). Its figures are hand-researched with an "as of" date. Update that date whenever you change a number.
- **No templating.** The header/nav (three vendor dropdowns, the AI District Braintrust link, and an external MLPerf Dashboard link) and the footer are copy-pasted into all 17 HTML files. Adding a page or changing the nav means editing every file.
- **Cache-busting.** Every page links `css/styles.css?v=N`. When you change the CSS, bump `N` in all 17 files so they stay the same.
- **Styling.** Everything is in `css/styles.css`. The Figtree font is self-hosted from `assets/fonts/Figtree font/` (the directory name contains a space). The layout is mobile-optimized, so check narrow widths after layout changes.

## News feed (`news.md`)

The "Neo-Cloud News" section on `index.html` is rendered on the client from `news.md` by `marked@12` (loaded from jsDelivr) and an inline script. The script relies on these Markdown conventions:

- Each `## Heading` (vendor name) starts a new `.news-card`. `###` subheadings (for example "Press releases" or "Blog") stay inside the current card.
- Each list item starts with a bold date, `- **Mon D, YYYY:** text`. The script styles the first `<strong>` in an `<li>` as `.news-date`.
- The script rewrites every link to open in a new tab. Add `[Source](url)` links in plain Markdown.
- List items are newest first.

To update the news, edit `news.md` only. The HTML doesn't need to change.
