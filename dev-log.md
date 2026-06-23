# Dev Log

A running log of activities and decisions for this site. Captures *why* things are
the way they are, not just *what* changed (git history covers the *what*).

**This log is append-only and chronological** — oldest entries first, new entries
added at the **bottom**. Don't rewrite or reorder past entries; appending keeps the
git history clean.

---

## Conventions

- **Editing content**: change `_data/*.json`; don't hardcode into templates.
- **Local dev**: `npm run dev` → http://localhost:8080 (watches CSS + templates).
- **Build**: `npm run build` → outputs to `_site/`.
- **Deploy**: push to `main` → Cloudflare Pages auto-builds.
- **Commits**: keep logical steps separate (scaffold vs. design were two commits).
- **This log**: append an entry at the bottom when wrapping up a work session —
  capture the *why* behind decisions, not just the *what*.

---

## Log

### 2026-06-21 — Initial scaffold

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

### 2026-06-21 — Visual refinement ("tech / futuristic")

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

### 2026-06-23 — Dev log + CLAUDE.md added; site live

Commits: `8d68838` (initial dev log), plus this restructure.

- Site is **live on Cloudflare Pages** as of this date (Will pushed to GitHub and
  connected the repo).
- Created this dev log. Originally written newest-first; **switched to append-only,
  chronological (oldest→newest)** so future entries append at the bottom and keep
  git diffs clean. Also renamed `dev-logs.md` → `dev-log.md` (singular).
- Added a `CLAUDE.md` capturing steady-state project instructions (goal,
  JSON-content convention, pure-HTML output, electric-blue palette, stack, dev and
  deploy commands), pointing here for history.
- Seeded file-based Claude Code memory with a user-profile note (local to this
  machine, separate from the Claude desktop app's account memory).

### 2026-06-23 — First real Experiment surfaced (Conway), build fix

Added the first published experiment to the site: **Conway** (Conway's Game of
Life) under Experiments → Algorithmic Visuals (`code-art`).

- **Decoupling decision.** Each experiment stays in its own repo with its own
  history and its own deploy; this site only holds a *catalog entry* that links
  out. Conway is deployed independently as a Cloudflare Worker
  (`conway.taj-tek.workers.dev`); we link to it rather than vendoring code.
  Rejected git submodules (couples the build) and copying built assets (artifact
  duplication / drift). The "content lives in JSON" convention makes this a pure
  `_data/experiments.json` edit — added one item to the `code-art` section.
- **Template upgrade** (`experiments/code-art.njk`): item rows are now fully
  clickable `<a>` rows (matching the index/ai-art pages) instead of just the
  arrow. External links (detected via `"://" in item.href`) open in a new tab
  with `target="_blank" rel="noopener"` and show a `↗` marker; internal links
  keep `→`. The no-`href` fallback row is preserved.
- **Build fix.** `CLAUDE.md` and `dev-log.md` were being picked up by Eleventy as
  Nunjucks templates and broke the build on the literal `{% include ... with %}`
  text inside them (pre-existing, since commit `cca7678`). Added both to
  `.eleventyignore` alongside `README.md`. Build is green again (6 files).
