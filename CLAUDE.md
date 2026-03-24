upda# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Stack

Jekyll static site with the Hydejack theme (v8.4.0), deployed to GitHub Pages. Uses npm/Webpack/Babel for JS and SCSS compilation alongside Ruby/Bundler for Jekyll.

## Common Commands

```bash
# Local development (Jekyll server + Webpack watch in parallel)
npm run dev

# Build for production (JS + CSS)
npm run build

# Jekyll only
bundle exec jekyll serve --port=4000

# Watch JS only
npm run watch:js

# Watch CSS only
npm run watch:css

# Format JS source
npm run format

# Regenerate resume YAML from JSON source
npm run resume   # json2yaml _data/resume.json > _data/resume.yml
```

## Architecture

### Content vs Theme separation

The site is a customized Hydejack theme, not a separate theme+content repo. Theme files (`_layouts/`, `_includes/`, `_sass/`, `_js/`) live alongside content, so edits to layouts directly affect the site.

### JavaScript pipeline

- Source: `_js/src/index.js` (entry point)
- Output: `assets/js/hydejack-{version}.js` (compiled by Webpack)
- Babel transpiles via `.babelrc` (`@babel/preset-env`)
- Never edit compiled files in `assets/js/` — edit `_js/src/` instead

### Multilingual structure

Content exists in parallel EN/TR versions:
- `blog/_posts/` / `blog-tr/_posts/`
- `podcast/_posts/` covers both; `podcast-en/` and `podcast/` are index pages
- `science/` has `en.md` and `tr.md`
- UI strings are in `_data/strings.yml`

When adding new content, check whether a TR version is needed.

### Collections

Defined in `_config.yml`: `featured_categories`, `featured_tags`, `projects`. Category/tag definitions live in `_featured_categories/` and `_featured_tags/`.

### Data files

- `_data/authors.yml` — author profiles (bio, social links per language)
- `_data/resume.yml` — generated from `_data/resume.json` via `npm run resume`
- `_data/social.yml` — social media links
- `_data/strings.yml` — all UI text (multilingual)

### Deployment

Push to `master` triggers `.github/workflows/deploy.yml`, which builds with `helaili/jekyll-action@v2` and pushes to the `gh-pages` branch. Do not push compiled assets manually; the CI handles it.
