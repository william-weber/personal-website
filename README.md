# personal-website

Static personal site. Built with [Eleventy](https://www.11ty.dev/) + [Tailwind CSS v4](https://tailwindcss.com/).

## Stack

- **SSG:** Eleventy 3 (Nunjucks templates, JSON data layer)
- **Styles:** Tailwind v4 CLI, no PostCSS config, no custom CSS beyond `@theme` tokens
- **Output:** plain static HTML in `_site/`
- **Host:** Cloudflare Pages

## Local dev

```sh
npm install
npm run dev          # serves at http://localhost:8080, watches CSS + templates
```

## Build

```sh
npm run build        # outputs to _site/
```

## Editing content

All editable content lives in `_data/` as JSON:

- `_data/site.json` — title, tagline, nav, footer
- `_data/about.json` — about-page bio + links
- `_data/experiments.json` — experiment subsections + items
- `_data/projects.json` — big-projects list (currently stubbed)

Layouts and partials live in `_includes/`:

- `_includes/layouts/base.njk` — page shell
- `_includes/partials/header.njk` — top nav (driven by `site.nav`)
- `_includes/partials/footer.njk` — footer (driven by `site.footer`)

Page templates are at the repo root and in `experiments/`.

## Deploy (Cloudflare Pages)

1. Push to GitHub.
2. Cloudflare → Pages → connect repo.
3. Build command: `npm run build`
4. Output directory: `_site`
