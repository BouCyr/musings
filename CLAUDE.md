# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

**Musings** is a personal writing site &mdash; short essays on various topics &mdash;
published as a static website via GitHub Pages. There is no build step, no
framework, and no dependencies: the site is hand-written HTML served as-is.

## Layout

- `index.html` &mdash; the welcome page: a tagline plus a hand-maintained list of
  articles, each with a one-line summary.
- `articles/*.html` &mdash; one file per piece. Filenames are slugs
  (e.g. `on-keeping-a-notebook.html`).
- `assets/style.css` &mdash; the single shared stylesheet for every page.
- `.github/workflows/pages.yml` &mdash; GitHub Pages deploy workflow.
- `.nojekyll` &mdash; tells Pages to serve files raw, skipping Jekyll processing.

## Adding an article

1. Copy an existing file in `articles/` to a new slug and edit the content,
   `<title>`, and `.meta` date.
2. Link to it from the `<ul class="index">` list in `index.html`, including a
   one-line `.summary`.

Article pages link back to the index and to the stylesheet with relative paths
(`../index.html`, `../assets/style.css`); the index uses non-prefixed paths
(`articles/...`, `assets/...`). Keep these relative so the site works both
locally and under the Pages base path.

## Publishing

Pushing to `main` triggers `.github/workflows/pages.yml`, which uploads the repo
root as the Pages artifact and deploys it. The repository's Pages source must be
set to **GitHub Actions** (Settings &rarr; Pages) for the workflow to publish.

## Local preview

Open `index.html` directly in a browser, or serve the root over HTTP:

```sh
python3 -m http.server
```
