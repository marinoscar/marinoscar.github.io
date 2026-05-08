## CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Oscar Marin's personal website (https://marin.cr), a Jekyll site built on the **Beautiful Jekyll** theme (v6.0.1) and deployed via GitHub Pages. The repo includes the entire theme source (`_layouts/`, `_includes/`, `assets/`, `beautiful-jekyll-theme.gemspec`) — it is not consumed as a remote theme — so theme files can be edited directly. `CNAME` maps the site to `marin.cr`.

## Local development

```bash
bundle install                          # first-time setup
bundle exec jekyll serve                # local server at http://localhost:4000
bundle exec jekyll build                # one-off build to ./_site
```

CI (`.github/workflows/ci.yml`) builds with `bundle exec appraisal jekyll build --future --config _config_ci.yml,_config.yml` on Ruby 3.3 and uploads the Pages artifact. The `--future` flag matters: it lets posts dated in the future build (some posts in `_posts/` are dated past today).

The `Gemfile` adds `github-pages` and `jemoji` on top of the gemspec. Don't remove `github-pages` — Pages uses its pinned gem versions.

## Content layout

- `index.md` → home page (`layout: home`), short personal intro.
- `aboutme.md` → still the upstream Beautiful Jekyll placeholder ("Inigo Montoya"). If editing personal content, this is the page that should be replaced, not `index.md`'s About section.
- `blog/index.html` → paginated post listing (5 per page, `paginate_path: blog/page:num`).
- `_posts/YYYY-MM-DD-slug.md` → blog posts. Permalink format is `/:year-:month-:day-:title/` (set in `_config.yml`), so the filename date is part of the public URL — renaming a post breaks its link.
- `food/index.md` → curated restaurant list. Each restaurant heading uses **Kramdown custom IDs** (`### 🍽️ [Name](url) {#slug}`) so anchors stay stable even when emojis or names change. Preserve this pattern when adding entries; recent commits (`e7b7795`) standardized it.
- `server/index.md` → dashboard of self-hosted services on `*.marin.cr`.
- `tags.html` → auto-generated tag index from post front-matter `tags: [...]`.

## Post front-matter conventions

Existing posts in `_posts/` follow this shape — match it for new posts:

```yaml
---
layout: post
title: "..."
subtitle: ...
cover-img: /assets/img/<slug>.png
thumbnail-img: /assets/img/<slug>.png
share-img: /assets/img/<slug>.png
tags: [tag-one, tag-two]
author: Oscar Marin
---
```

Defaults in `_config.yml` already set `layout: post`, `comments: true`, and `social-share: true` for everything in `_posts/`, so those don't need to be repeated. Non-post pages default to `layout: page`.

## Markdown / rendering

- Markdown processor: `kramdown` with `input: GFM` and `rouge` highlighter.
- `{#custom-id}` after a heading sets the anchor — used heavily in `food/index.md`. Use it when a heading contains emoji or punctuation that would otherwise produce an ugly auto-slug.
- Site timezone is `America/Chicago`.

## Things to leave alone

- `CHANGELOG.md`, `LICENSE`, `screenshot.png`, `Appraisals`, `beautiful-jekyll-theme.gemspec`, and most of `_layouts/` + `_includes/` are upstream Beautiful Jekyll. Don't reformat or "clean them up" unless the change is intentional — they're tracked so the theme can be updated by merging upstream.
- `staticman.yml` is configured but the `staticman:` block in `_config.yml` is commented out, so comments are not actually wired up. Don't assume comments work end-to-end without enabling that block.
