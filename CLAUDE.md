# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

**Musings** is a personal writing site &mdash; short essays on various topics &mdash;
published as a static website via GitHub Pages. There is no build step, no
framework, and no dependencies: the site is hand-written HTML served as-is.

## Layout

- `index.html`, `staging.html` &mdash; **generated** listing pages. Do not edit
  by hand; they are rebuilt by `scripts/build_index.py`. `index.html` lists only
  published articles; `staging.html` is the same page listing every article.
- `articles/*.html` &mdash; one file per piece. Filenames are slugs
  (e.g. `on-keeping-a-notebook.html`). This is the only hand-edited content.
- `scripts/build_index.py` &mdash; scans `articles/` and regenerates the two
  listing pages. Pure Python stdlib, no dependencies.
- `assets/style.css` &mdash; the single shared stylesheet for every page.
- `.github/workflows/pages.yml` &mdash; GitHub Pages deploy workflow.
- `.nojekyll` &mdash; tells Pages to serve files raw, skipping Jekyll processing.

## Adding an article

1. Copy an existing file in `articles/` to a new slug and edit the body,
   `<title>`, and `.meta` date.
2. Regenerate the listings: `python3 scripts/build_index.py`. There is no manual
   list to maintain &mdash; the index/staging pages are derived from the articles.

The generator reads each article as follows:

- **Title** &mdash; the first `<h1>`.
- **Summary** (shown on the listings) &mdash; the first `<p>` whose class list
  contains `summary` (other classes are allowed; if several match, the first
  wins). Articles without one get an empty summary.
- **Date / ordering** &mdash; the first `<p class="meta">`, parsed as
  `%d %B %Y` (e.g. `23 June 2026`). Listings are sorted most-recent-first;
  unparseable/missing dates sort last.
- **Published** &mdash; the article appears on `index.html` only if it contains
  `<meta name="status" content="published">`. Without it, the article still
  appears on `staging.html` but is treated as a draft.

Article pages link back to the index and to the stylesheet with relative paths
(`../index.html`, `../assets/style.css`); the generated listings use
non-prefixed paths (`articles/...`, `assets/...`). Keep these relative so the
site works both locally and under the Pages base path.

## Publishing

Pushing to `main` triggers `.github/workflows/pages.yml`, which runs
`scripts/build_index.py` to rebuild the listings, then uploads the repo root as
the Pages artifact and deploys it. The repository's Pages source must be set to
**GitHub Actions** (Settings &rarr; Pages) for the workflow to publish.

Note `staging.html` ships to the live site too (it is just an unlinked page);
it is reachable by URL but not linked from `index.html`.

## Local preview

Open `index.html` directly in a browser, or serve the root over HTTP:

```sh
python3 -m http.server
```
