# hrydi.github.io

Personal blog & portfolio milik [hrydi](https://github.com/hrydi), dibangun pakai [Astro](https://astro.build). Live di **https://hrydi.github.io**.

Isinya seputar PHP, Node.js, Golang, dan Python — sharing pengalaman, tips, dan eksperimen ngoding, plus showcase project-project yang pernah dibikin.

## Stack

- **Astro 7** + **MDX** — static site generation
- **Content Collections** (`blog` & `portfolio`), schema-nya ada di [src/content.config.ts](src/content.config.ts)
- **@astrojs/sitemap** — generate `sitemap-index.xml` otomatis
- **@astrojs/rss** — RSS feed di `/rss.xml`
- Deploy otomatis ke **GitHub Pages** lewat GitHub Actions ([.github/workflows/astro.yml](.github/workflows/astro.yml)) tiap push ke branch `master`

## 🚀 Struktur Project

```text
├── public/                 # static assets (gambar, favicon, robots.txt)
├── src/
│   ├── components/         # BaseHead, Header, Footer, dll.
│   ├── content/
│   │   ├── blog/           # tulisan blog (Markdown)
│   │   └── portfolio/      # showcase project (Markdown)
│   ├── content.config.ts   # schema frontmatter tiap collection
│   ├── layouts/            # BlogPost.astro
│   └── pages/              # routes: /, /blog, /portfolio, /about, /rss.xml
├── astro.config.mjs
└── package.json
```

Astro bakal generate route otomatis dari file di `src/pages/`. Konten blog & portfolio dikelola lewat [Content Collections](https://docs.astro.build/en/guides/content-collections/), jadi frontmatter-nya type-checked sesuai schema di `src/content.config.ts`.

## ✍️ Nulis Post Baru

**Blog** — bikin file `.md` baru di `src/content/blog/` dengan frontmatter:

```yaml
---
title: 'Judul Post'
description: 'Deskripsi singkat buat meta tag & SEO'
pubDate: 'Jul 19 2026'
heroImage: '/nama-gambar.png' # opsional
---
```

**Portfolio** — bikin file `.md` baru di `src/content/portfolio/`, ada field tambahan: `tags`, `liveUrl`, `repoUrl`.

## 🧞 Commands

Semua command dijalankan dari root project:

| Command                   | Action                                            |
| :------------------------ | :------------------------------------------------ |
| `npm install`              | Install dependencies                              |
| `npm run dev`               | Jalanin dev server lokal di `localhost:4321`       |
| `npm run build`             | Build production site ke `./dist/` (jalanin `astro check` dulu) |
| `npm run preview`           | Preview hasil build lokal sebelum deploy           |
| `npm run astro ...`         | Jalanin CLI command Astro, misal `astro add`       |

## 🚢 Deployment

Push ke branch `master` otomatis trigger workflow [astro.yml](.github/workflows/astro.yml): build situs lalu deploy ke GitHub Pages. Nggak perlu langkah manual tambahan.

## Credit

Tema awal berdasarkan [Astro Blog Starter Kit](https://github.com/withastro/astro/tree/latest/examples/blog), yang terinspirasi dari [Bear Blog](https://github.com/HermanMartinus/bearblog/).
