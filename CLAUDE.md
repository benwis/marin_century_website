# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static website for the Marin Century cycling event, built with **Zola** (Rust-based static site generator) and deployed on **Fly.io**. The site provides event information, route details, registration links, and sponsor visibility for a premier century bike ride in Northern California.

## Build & Deploy

```bash
# Build the site (compiles SCSS, generates search index, outputs to /public)
zola build

# Local development server with live reload
zola serve
```

Deployment is automated via GitHub Actions (`.github/workflows/fly-deploy.yml`) — pushing to `main` triggers a Fly.io deploy. The Docker build uses a two-stage process: Zola builds the site, then `joseluisq/static-web-server` serves it.

## Architecture

- **Templating**: Tera templates in `templates/`. `base.html` is the layout wrapper; `index.html` is the main single-page site; `quiz_template.html` handles the route selection quiz.
- **Styling**: Single SCSS file at `sass/style.scss` compiled by Zola. Uses CSS custom properties for theming (colors, fonts, sizes) and BEM-style class naming (`header__logo-mobile`, `route-card`, etc.).
- **Content**: Markdown files in `content/` processed by Zola. Currently minimal — most content lives directly in templates.
- **Static assets**: Images in `static/img/`.
- **Config**: `config.toml` (Zola settings), `fly.toml` (Fly.io deployment), `Dockerfile` (multi-stage build).

## Key Integrations

- **RedPodium**: External registration system. Registration links are dynamically updated with UTM parameters via a JavaScript MutationObserver in `base.html`.
- **RideWithGPS**: Route maps embedded as iframes.
- **Google Analytics (GA4)**: Tracking in `base.html` with ID `G-D8T7SPST03`.
- **UTM tracking script**: Captures URL marketing parameters and appends them to all RedPodium registration links, ensuring attribution through the registration flow.

## Zola Specifics

- Zola version: **0.17.2** (pinned in Dockerfile)
- Sass compilation enabled (`compile_sass = true` in config.toml)
- Search index generation enabled but not prominently used
- Base URL toggles between `marin-century.fly.dev` and `marincentury.com` in config.toml
