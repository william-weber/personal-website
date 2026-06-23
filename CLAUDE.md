# CLAUDE.md

Guidance for working in this repo. For the chronological history of decisions, see
[`dev-log.md`](./dev-log.md).

## What this is

Will's personal website — an About + Experiments + Projects site, shipped as a
minimal v1 and grown over time. Kept deliberately simple.

## Core principles

- **Content lives in JSON, not templates.** All editable copy is in `_data/*.json`
  (`site`, `about`, `experiments`, `projects`). To change wording, nav, or footer,
  edit the JSON — don't hardcode strings into `.njk` files. This is the main
  convention; respect it.
- **Compiles down to pure static HTML.** Eleventy renders plain HTML at build time;
  the shipped site has effectively no client JS (the one exception is a tiny inline
  `setInterval` for the telemetry clock). Don't introduce a client-side framework or
  runtime JS without good reason.
- **Electric-blue color scheme.** Saturated blue base (`#0c1638`), single accent
  `#7aa0ff`, on a dark theme. Colors are defined as `--color-*` tokens in
  `src/styles.css` `@theme`; use those tokens, don't hardcode new hex values.
- **Tailwind only.** Style with Tailwind utility classes + the `--color-*` tokens.
  No custom CSS beyond the `@theme` token block and the few keyframes/utilities
  already in `src/styles.css`.

## Stack

- **Eleventy (11ty)** static site generator, **Nunjucks (.njk)** templates.
- **Tailwind v4 CLI** (standalone, no PostCSS).
- Shared `_includes/layouts/base.njk` + `header`/`footer`/`telemetry` partials; nav
  is driven by `_data/site.json` so there's no per-page drift.
- Reusable bits like the section marker are Nunjucks **macros**, imported per page —
  note `{% include ... with %}` is **not** supported in Nunjucks.

## Commands

- `npm run dev` — local server at http://localhost:8080, watches CSS + templates.
- `npm run build` — outputs static site to `_site/`.

## Deploy

Push to `main` → **Cloudflare Pages** auto-builds (build `npm run build`, output
`_site/`).

## Conventions

- Keep commits as separate logical steps.
- Append a `dev-log.md` entry (at the bottom) when wrapping up a work session —
  record the *why*, not just the *what*.
