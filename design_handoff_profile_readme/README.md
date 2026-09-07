# Handoff: GitHub Profile README — vladhrusha (option 1b, "table-driven")

## Overview
A redesign of the GitHub **profile README** rendered at `github.com/vladhrusha` (source: the
`vladhrusha/vladhrusha` repo, `README.md` on `main`). The current README stacks each technology on
its own row with inline CSS that GitHub silently strips, so it renders as a very tall, unstyled list.

Option **1b** restructures it: a two-column intro (bio left, illustration right), the tech stack as a
**4-column table of icon + label cells**, and a **Selected work** table. Designed under a hard
constraint: *only markup GitHub's markdown pipeline actually renders.*

## About the Design Files
The files in this bundle are **design references created in HTML** — a prototype showing the intended
look and structure, not production code to copy.

The unusual part of this handoff: the **target "codebase" is a single `README.md` file** rendered by
GitHub's markdown pipeline. So the implementation is not a React app — it is markdown + the narrow
subset of HTML GitHub permits. `README.profile.md` in this bundle is a ready-to-commit
implementation; treat the HTML/screenshots as the spec it must match.

### GitHub markdown constraints (critical)
GitHub sanitizes README HTML. What **works**:
- `<h1>`–`<h6>`, `<p>`, `<hr>`, `<br>`, `<strong>`, `<em>`, `<code>`, `<a>`, `<ul>`/`<ol>`
- `<table>`, `<thead>`, `<tbody>`, `<tr>`, `<th>`, `<td>` — and markdown pipe tables
- `<img>` with `src`, `alt`, `width`, `height`, `align`
- `align="center"` / `align="right"` on block elements and `<img>`
- `<picture>` + `<source media="(prefers-color-scheme: dark)">` for theme-swapped images
- `<details>` / `<summary>` for collapsibles

What is **stripped or ignored**:
- **`style` attributes** (the current README's `display:flex`, `font-size`, `column-gap` all do nothing)
- `class`, `id`, `<style>` blocks, `<script>`
- CSS custom properties, media queries, hover states, transitions

Consequence: the layout must be carried by **tables, `align`, and image sizing** alone.

## Fidelity
**High-fidelity** for type scale, structure, spacing intent, and content. But GitHub owns the final
typography and colors — it renders the README in its own `markdown-body` styles. The hex values in
this doc document *what the mock assumed GitHub renders* (so a reviewer can compare screenshots to
the real page); they are **not** values to author. Only structure and content are yours to control.

## Screens / Views

### Screen: Profile README (single view, two themes)
**Purpose:** A visitor landing on the profile learns in ~5 seconds who Vlad is, what he works in, and
what to look at next.

**Layout** (content column is GitHub's, ~880px max at desktop width):

1. **Intro band** — two columns. Left: name, role line, 2-sentence bio. Right: illustration at
   `width="230"`, top-aligned, ~28px gutter. Implemented as a 2-cell table row (`<td>` widths ~72% / 28%)
   or `<img align="right">` before the text.
2. **Rule** — `<hr>`, ~26px above / 18px below.
3. **`Languages and Tools`** — `<h3>`, then a **4-column × 2-row table**, 8 cells. Each cell:
   34px icon centered, 7px gap, label below at 13px muted. Cells are equal width (25%).
   Checkerboard cell backgrounds (see tokens) — *note: GitHub applies its own zebra striping to table
   rows, not cells, so the checkerboard is a mock-only nicety; do not fight it.*
   The 8th cell is the placeholder `+ next up` in monospace.
4. **`Selected work`** — `<h3>`, then a 3-column table: `Project` (repo link) | `What it is` (one line) |
   `Stack` (~150px). Three rows.

**Components**

| Component | Spec |
| --- | --- |
| `h1` name | 30px / 600 / line-height 1.2 / letter-spacing −0.4px. Content: `Vladyslav Hrusha` |
| Role line | 16px / 400 / muted. Content: `Frontend Developer — React & Next.js, MERN stack` |
| Bio | 15px / 400 / line-height 1.6, `text-wrap: pretty`, max ~60ch |
| Illustration | `assets/main.png`, `width="230"`, `align="right"`, `alt="illustration"` |
| `hr` | 1px solid border color, no shadow |
| `h3` section head | 17px / 600, 26px top margin, 13px bottom |
| Tech cell | 14px 8px padding, 1px border, centered; icon 34×34; label 13px muted |
| Table head cell | 8px 13px padding, 600 weight, subtle fill background |
| Table body cell | 8px 13px padding, 14px / line-height 1.5 |
| Repo link | 500 weight, link color, `text-decoration: none` in mock — GitHub underlines on hover |
| Placeholder text | `ui-monospace, Menlo, monospace`, 13px, muted — **all placeholder copy must be replaced** |

**Exact copy used in the mock**
- H1: `Vladyslav Hrusha`
- Role: `Frontend Developer — React & Next.js, MERN stack`
- Bio: `I build fast, accessible product front-ends and the Node services behind them. Currently deep in Next.js App Router, TypeScript and design-system work.`
- Section heads: `Languages and Tools`, `Selected work`
- Tech labels: `React`, `Next.js`, `TypeScript`, `JavaScript`, `Node.js`, `Redux`, `MaterialUI`, `+ next up`
- Table heads: `Project`, `What it is`, `Stack`
- Rows: `lifely` / `findbait_ai` / `portfolio`, each with `one line on what it does →` (PLACEHOLDER)
  and stacks `Next · Node · Mongo`, `React · TS`, `Next · MUI`

## Interactions & Behavior
Effectively none — a README is a static document. The only live behaviors:
- **Links** — repo names link to their GitHub repos; social links to external profiles. GitHub styles
  and underlines these itself.
- **Theme** — the page follows the viewer's GitHub theme (dark/light/dimmed). You do **not** get a
  media query, so any image that must change per theme uses `<picture>` + `<source media="(prefers-color-scheme: dark)">`.
- **Responsive** — GitHub's content column narrows on mobile and tables become horizontally
  scrollable. The 4-column tech table is the risk area: verify it on a phone; if it squeezes, drop to
  a 3-column × 3-row table.

## State Management
None. Static document, no state, no data fetching.

If dynamic stat cards are added later (they are toggled off in option 1b, present in option 1c), they
are **externally rendered images** — e.g. `github-readme-stats` — embedded as `<img src="https://…">`
and cached by GitHub's image proxy. No client code either way.

## Design Tokens
Documented so screenshots can be compared to the live page. GitHub supplies these; don't author them.

**Dark theme**
| Token | Value |
| --- | --- |
| Canvas / page bg | `#0d1117` |
| Subtle fill (table head, alt cell) | `#151b23` |
| Border | `#3d444d` |
| Text primary | `#e6edf3` |
| Text muted | `#9198a1` |
| Text faint (placeholder) | `#656c76` |
| Link | `#4493f8` |

**Light theme**
| Token | Value |
| --- | --- |
| Canvas / page bg | `#ffffff` |
| Subtle fill | `#f6f8fa` |
| Border | `#d1d9e0` |
| Text primary | `#1f2328` |
| Text muted | `#59636e` |
| Text faint (placeholder) | `#818b98` |
| Link | `#0969da` |

**Typography** — `-apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif`;
monospace `ui-monospace, Menlo, monospace`.
Scale: 30px/600 (h1) · 17px/600 (h3) · 16px/400 (role) · 15px/400 (bio) · 14px/400 (table body) ·
13px/400 (tech label, mono placeholder) · 12px (mono micro).

**Spacing** — 6 · 7 · 8 · 13 · 14 · 18 · 22 · 26 · 28 · 34 · 40px. Card padding `32px 40px 36px`.

**Radius** — 6px (containers), 4px (chips). **Shadows** — none; borders only.

## Assets
All pulled from the source repo at `vladhrusha/vladhrusha@main`, under `public/`:

| File | Bundled at | Notes |
| --- | --- | --- |
| `public/main.png` | `assets/main.png` | 1712×1354 flat vector illustration (browser/team scene). **Not a portrait** — a generic stock illustration. Consider replacing with a real photo or dropping it. |
| `public/icons/react.png` | `assets/icons/react.png` | |
| `public/icons/nextjs.png` | `assets/icons/nextjs.png` | **Contrast issue:** black circular mark, nearly invisible on the `#0d1117` dark canvas. Swap for a white/inverted variant, or use `<picture>` with a per-theme source. |
| `public/icons/typescript.png` | `assets/icons/typescript.png` | |
| `public/icons/javascript.png` | `assets/icons/javascript.png` | |
| `public/icons/nodejs.png` | `assets/icons/nodejs.png` | **Contrast issue:** dark green wordmark, low contrast on dark. Same fix. |
| `public/icons/redux.svg` | `assets/icons/redux.svg` | |
| `public/icons/materialui.svg` | `assets/icons/materialui.svg` | |

Icons must be committed in the repo and referenced by **relative path** (`public/icons/react.png`) —
that works on the profile README because it is rendered from its own repo.

## Files
| File | What it is |
| --- | --- |
| `README.profile.md` | **Ready-to-commit implementation** of 1b. Copy over the repo's `README.md`. |
| `Profile README.dc.html` | The design prototype. Contains three options; **1b is the one to build** (`id="1b"`). Options 1a and 1c are context. |
| `screenshots/1b-dark.png` | 1b rendered in dark theme (2× / 1640×1442) |
| `screenshots/1b-light.png` | 1b rendered in light theme (2× / 1640×1442) |
| `assets/` | Icons and illustration, as bundled above |

## Open items for the developer / owner
1. **Replace the three `one line on what it does →` placeholders** with real project descriptions.
2. **Confirm the repo links** — `lifely`, `findbait_ai` and `portfolio` are currently **private**; a
   private repo link 404s for visitors. Either make them public or link to live deployments instead.
3. **Fix the two low-contrast icons** (Next.js, Node.js) for dark theme.
4. **Decide on the illustration** — keep, replace with a photo, or drop.
5. **Verify the 4-column tech table on mobile**; fall back to 3 columns if it crowds.
