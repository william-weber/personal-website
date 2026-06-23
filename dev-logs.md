# Dev Logs

A running log of activities and decisions for this site. Newest entries at the top.
Intended as a memory aid across work sessions — capturing *why* things are the
way they are, not just *what* changed (the git history covers the *what*).

---

## 2026-06-23 — Dev log created

Started keeping this log. Backfilled the entries below from the first two work
sessions. Site is live on Cloudflare Pages as of this date.

---

## 2026-06-21 — Visual refinement ("tech / futuristic")

Commit: `6292fb6` — *Apply tech/futuristic visual refinement*

Took the scaffold's design over to Claude Design, which produced a high-fidelity
HTML prototype + handoff doc (a "tech / instrument-panel" direction). Applied that
refinement to the Eleventy codebase. IA, content, and routing were left unchanged —
this was a visual-system change only.

What changed:
- **Palette**: swapped near-black for a saturated electric blue base (`#0c1638`),
  single accent `#7aa0ff`, plus status colors (online green, in-progress amber).
- **Typography**: Geist + Geist Mono from Google Fonts; thin-weight (100) display
  headings.
- **Background**: composite of a 32px dot grid + two ambient radial glows.
- **New chrome**: telemetry strip above the header (pulse dot, coords, version,
  live UTC clock), 44px indexed-nav header with active-page underline, 40px mono
  footer with a blinking "end of transmission" cursor, corner crosshairs inside
  `<main>`.
- **Pages**: home (hero + contents card grid + "now" strip), about (hero + mono
  meta block), experiments (hairline list rows), projects (2-up status cards).
  Experiment leaf pages got a matching "standby" empty state.

Decisions / notes:
- **Ported inline styles → Tailwind utilities + CSS custom properties.** The
  prototype was all inline styles; per the handoff we kept the existing Tailwind +
  `--color-*` token approach rather than copying inline styles verbatim.
- **Nunjucks `{% include ... with %}` is not supported.** First attempt at a
  parameterized section-marker partial failed the build. Switched to a Nunjucks
  **macro** (`_includes/partials/section-marker.njk`, imported per page) instead.
- **Live UTC clock** in the telemetry strip is the one bit of client JS — a tiny
  inline `setInterval`. Everything else is static.
- Verified all six routes render (headless Chrome screenshots) before committing.

---

## 2026-06-21 — Initial scaffold

Commit: `35fe18b` — *Scaffold personal site with Eleventy + Tailwind v4*

First build-out of the site. Context: Will has wanted a personal site for years but
kept stalling — partly perfectionism (waiting for the bigger projects to be
"ready"). Decision was to **ship a minimal v1 now** and treat the site itself as an
ongoing project, rather than gate it on other work.

Stack decisions:
- **Eleventy (11ty)** as the static site generator. Chosen over Astro (component
  overhead), Hugo (Go templates), Jekyll (Ruby toolchain), and plain Vite+includes
  (reinvents the wheel). 11ty gives plain static HTML output, a first-class JSON
  data layer, zero client JS by default, and Nunjucks templates for shared
  layout/partials — directly satisfying the "pure HTML feel, content in JSON, no
  hardcoded nav/footer" goals.
- **Tailwind v4 CLI** (standalone, no PostCSS config). No custom CSS beyond
  `@theme` color tokens.
- **Nunjucks (.njk)** templates.
- **Dark theme** by default (no toggle in v1).
- **Cloudflare Pages** as the host (build `npm run build`, output `_site/`).

Structure:
- Content lives in `_data/*.json` (`site`, `about`, `experiments`, `projects`) so
  edits don't touch templates.
- Shared `_includes/layouts/base.njk` + `header`/`footer` partials, nav driven by
  `site.json` — avoids per-page drift.
- Pages: home, about, experiments index + `ai-art`/`code-art` subsections, and a
  **stubbed** projects page (the two big projects — Auspicious Times, VR Phone
  Tracker — are still in-progress and intentionally shown as such rather than
  hidden or fully detailed).

---

## Conventions

- **Editing content**: change `_data/*.json`; don't hardcode into templates.
- **Local dev**: `npm run dev` → http://localhost:8080 (watches CSS + templates).
- **Build**: `npm run build` → outputs to `_site/`.
- **Deploy**: push to `main` → Cloudflare Pages auto-builds.
- **Commits**: keep logical steps separate (scaffold vs. design were two commits).
- **This log**: add an entry when wrapping up a work session — capture the *why*
  behind decisions, not just the *what* (git covers the what). Newest entries on top.
