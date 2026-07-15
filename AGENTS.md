# hrydi.github.io — Agent Guide

## Project

Personal blog — Astro 7 static site, no framework (plain `.astro` components), deployed to GitHub Pages.

- **Base URL:** `https://hrydi.github.io` (set in `astro.config.mjs`)
- **Branch:** `master` (CI deploys on push)
- **CI:** `.github/workflows/astro.yml` — `withastro/action` + `actions/deploy-pages`

## Commands

| Command | What it does |
|---------|-------------|
| `npm run dev` | Dev server at `localhost:4321` |
| `npm run build` | **Typechecks then builds** — runs `astro check && astro build` |
| `npm run preview` | Preview production build |
| `npm run astro <cmd>` | Arbitrary Astro CLI |

No tests, no linter, no formatter configured.

## Architecture

- **Blog posts:** `src/content/blog/*.md` — loaded via Astro content collections with Zod schema (`src/content.config.ts`). Frontmatter: `title`, `description`, `pubDate` (Date), `updatedDate` (optional Date), `heroImage` (optional string).
- **Portfolio:** `src/content/portfolio/*.md` — same collection API. Frontmatter: `title`, `description`, `pubDate` (Date), `heroImage`, `tags` (string[]), `liveUrl`, `repoUrl` (all optional). Listing at `/portfolio`.
- **Routes:** `src/pages/` — `index.astro` (/), `blog/index.astro` (/blog), `blog/[...slug].astro` (/blog/:slug, dynamic via `getStaticPaths`), `about.astro`, `rss.xml.js`.
- **Layout:** `src/layouts/BlogPost.astro` wraps each post.
- **Constants:** `SITE_TITLE`, `SITE_DESCRIPTION` in `src/consts.ts`.
- **Styling:** `src/styles/global.css` (Bear Blog theme, no CSS framework).

## TypeScript

`tsconfig.json` extends `astro/tsconfigs/strict` with `strictNullChecks: true`. `astro check` is the typechecker (run via `npm run build`).

## Content conventions

- Blog markdown files use `.md`, placed directly in `src/content/blog/`.
- Blog listing sorts by `pubDate` descending.
- RSS feed at `/rss.xml`.
- Sitemap is auto-generated (`@astrojs/sitemap`).

## Known gaps

- No OG image file exists for `blog-placeholder-1.jpg` (referenced in `BaseHead.astro` default).
