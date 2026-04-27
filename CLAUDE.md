# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A personal blog built with **Hexo 7.2.0**, deployed to GitHub Pages at `77philosophia.github.io`. Content is bilingual (Chinese/English), covering ML/AI, programming, robotics, and personal notes (~218 posts).

## Build & Run Commands

```bash
hexo clean          # Clean generated files and cache
hexo generate       # Build static site to public/
hexo server         # Local dev server (default http://localhost:4000)
hexo deploy         # Deploy to GitHub Pages via git
hexo new "Title"    # Create a new post draft
```

All commands should be run from the repo root. `npm run build` / `npm run server` / `npm run deploy` are equivalent aliases.

## Resume System (RenderCV)

The blog includes a resume pipeline using [RenderCV](https://github.com/sinaatalay/rendercv), managed in Python:

```bash
source venv_cv/bin/activate
rendercv render resume_src/wangxin_zh.yaml   # Chinese resume
rendercv render resume_src/wangxin_en.yaml   # English resume
```

Pushing changes to `resume_src/` triggers a GitHub Actions workflow (`.github/workflows/render_resume.yml`) that renders both PDFs and commits them to `source/downloads/`.

## Architecture

- **`_config.yml`** — Hexo site config: URL, permalinks, deployment settings. Deployment target is `git@github.com:77philosophia/77philosophia.github.io.git` on branch `master`.
- **`themes/next/`** — NexT theme (Muse scheme). Theme config is at `themes/next/_config.yml`. Key features: MathJax enabled (requires `hexo-renderer-pandoc`), lazyload images, Noto Serif SC font.
- **`source/_posts/`** — All blog posts. Each post may have a companion asset folder (e.g., `Post-Title/` for images) since `post_asset_folder: true`.
- **`source/images/`** — Global site images (favicon, avatar, etc.).
- **`source/resume/`** — Resume page served at `/resume/`.
- **`scaffolds/`** — Post/page templates.

## Key Configuration

- **Renderer**: `hexo-renderer-pandoc` is used (not the default marked renderer), which is required for MathJax support.
- **Permalink format**: `:year/:month/:day/:title/`
- **Asset linking**: `hexo-asset-link` plugin handles post-relative asset references.
