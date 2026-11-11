# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page CV for Greatman Lim, built with Hugo + Dart Sass (for SCSS). Content is in Finnish. Deployed to Netlify at https://cv.mahti.org. Pinned to Hugo `0.162.1` and Dart Sass `1.107.1` (see `netlify.toml`, which installs Dart Sass before building).

## Commands

```bash
hugo server          # local dev with live reload (http://localhost:1313)
hugo --gc --minify   # production build into public/ (same command Netlify runs)
hugo new content/item/<slug>.md   # scaffold a new CV entry from archetypes/default.md
```

There are no tests, linters, or npm tooling — Hugo is the entire toolchain.

## Architecture

The whole site is rendered by **one template**, `layouts/index.html`. There are no other layouts, partials, or shortcodes. Understanding that file is understanding the site. It assembles two data sources:

1. **Timeline entries** — `content/item/*.md`. Each is a Markdown file with front matter (`title`, `startYear`, `endYear`, `description`, `categories: ["item"]`). The template ranges over them sorted by `startYear` descending. The Markdown body becomes the entry's prose.

2. **Sidebar facts** — `data/facts.json`. A list of labelled groups (Yhteystiedot, Taidot, Kielitaito, etc.), each with `items`. Per item: `fact` (may contain inline HTML, rendered via `markdownify`), optional `description`, `link`, and `printhref` (a URL shown only in print, hidden on screen via the `hide-on-print` class).

Styling is a single SCSS file, `assets/sass/index.scss`, compiled with Dart Sass (`css.Sass` with `transpiler: dartsass`; libsass is deprecated), minified, and fingerprinted by the template via Hugo Pipes. Local builds need a `sass` (Dart Sass) binary on PATH. The design has a dedicated **print stylesheet path** (classes like `hide-on-print`, `printhref`) — when editing layout or styles, check both screen and print rendering.

### Config intent (config.yaml)

The config is deliberately locked down for the single-page use case; preserve these when editing:
- `disableKinds` turns off taxonomies, sections, RSS, sitemap, and 404 so Hugo doesn't emit "no layout" warnings or stray pages.
- The `cascade` block sets `/item/**` to `render: never` — item pages are data consumed by `index.html`, never served as standalone URLs.
- `markup.goldmark.renderer.unsafe: true` is required because content and `facts.json` author inline HTML (mailto/anchor tags). Do not remove it.

## Adding/editing CV content

- New timeline entry: add a file under `content/item/`. Set `startYear` (and optionally `endYear`); these drive ordering and the displayed date range. New files from the archetype are `draft: true` — set `draft: false` to publish.
- New sidebar fact: edit `data/facts.json`.
