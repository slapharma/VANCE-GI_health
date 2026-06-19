# VANCE GI Health — Handover Document

**Date:** June 2026  
**Author:** Clifton Flack / SLA Pharma  
**Live URL:** https://vance-gi-health.vercel.app  
**GitHub:** https://github.com/slapharma/VANCE-GI_health  
**Vercel project:** `vance-gi-health` (team: SLA Pharma)

---

## What this is

A clean, embeddable widget for the Vance Health Hub covering seven common GI conditions. It is designed to be embedded as an iframe inside the main WordPress hub at vancehealthhub.co.uk — no site header, no site footer, no hero banner. The content was written and reviewed by Mia (Mia's revised copy, v2).

---

## Structure

```
/
├── index.html                   Landing page — 7-condition card grid + stat band + CTA
├── conditions/
│   ├── inflammatory-bowel-disease.html
│   ├── ulcerative-colitis.html
│   ├── crohns-disease.html
│   ├── microscopic-colitis.html
│   ├── irritable-bowel-syndrome.html
│   ├── colorectal-cancer.html
│   └── diverticular-disease.html
├── assets/
│   ├── css/gi-health.css        Single stylesheet (all components)
│   ├── js/gi-health.js          Reveal-on-scroll, counter animations, TOC highlighting
│   └── img/
│       ├── favicon.png
│       ├── logo.png
│       ├── icons/               SVG icons (one per condition + utility icons)
│       └── illustrations/       Sidebar header motifs (conditions-motif.svg, toc-motif.svg)
├── vercel.json                  trailingSlash: false
└── HANDOVER.md                  This file
```

---

## Conditions covered

| Page | File |
|------|------|
| Inflammatory Bowel Disease (IBD) | `conditions/inflammatory-bowel-disease.html` |
| Ulcerative Colitis | `conditions/ulcerative-colitis.html` |
| Crohn's Disease | `conditions/crohns-disease.html` |
| Microscopic Colitis | `conditions/microscopic-colitis.html` |
| Irritable Bowel Syndrome (IBS) | `conditions/irritable-bowel-syndrome.html` |
| Colorectal Cancer | `conditions/colorectal-cancer.html` |
| Diverticular Disease & Diverticulitis | `conditions/diverticular-disease.html` |

---

## Layout — condition pages

Each condition page uses a three-column CSS Grid:

```
[ GI Conditions sidebar ] [ Main content ] [ On this page TOC ]
       250px                minmax(0,1fr)         250px
```

**Responsive breakpoints:**
- `> 1080px` — all three columns
- `768px–1080px` — left sidebar + main only (TOC rail hidden)
- `< 768px` — single column, sidebar stacks above content

**Left sidebar (indication selector):** teal "GI Health Home" button + "GI Conditions" nav card listing all 7 conditions. The current condition gets `class="active"` on its link.

**Right rail:** "On this page" TOC with animated square-node stepper. JS auto-highlights the active section as you scroll.

**Page title strip (`.cond-page-title`):** replaces the old dark hero banner — plain white, condition name + one-line lede, teal bottom border.

---

## Key design tokens (CSS variables)

| Token | Value | Use |
|-------|-------|-----|
| `--primary-color` | `#008080` | Teal — links, borders, active states |
| `--primary-hover` | `#006666` | Darker teal on hover |
| `--primary-wash` | `#def4f4` | Pale teal backgrounds |
| `--secondary-color` | `#0A1929` | Navy — headings, TOC sidebar |
| `--container-width` | `1320px` | Max layout width |
| `--font-heading` | Outfit | All headings |
| `--font-main` | Inter | Body text |

All components use `border-radius: 0` (square corners throughout).

---

## Deployment

- **Platform:** Vercel, connected to GitHub (`slapharma/VANCE-GI_health`, `main` branch)
- **Auto-deploy:** every push to `main` triggers a new production deployment
- **No build step** — pure static HTML/CSS/JS, served as-is
- **DNS/domain:** currently on the Vercel-assigned domain; can be pointed to a custom domain via Vercel dashboard

To deploy a change: edit files, commit, push to `main`. Vercel picks it up within ~30 seconds.

---

## Content guidelines (Mia's version)

The copy follows these principles agreed during Mia's review:

- **Patient-facing tone** — plain language, no clinical jargon without explanation
- **Age framing** — age is not presented as a cause of conditions, only as context
- **Novelty stats removed** — no "surprising" or "little-known" statistics
- **SVG diagrams** — labelled anatomical SVGs replace bar-chart infographics (IBD types, UC extent, Crohn's locations)
- **Microscopic colitis** — includes a comparison table for collagenous vs lymphocytic subtypes
- **Colorectal cancer** — uses a screening-steps infographic and reassurance copy; survival-by-stage chart removed
- **Smoking note (IBD pages)** — explicitly clarifies the UC/Crohn's difference (smoking protective in UC, harmful in Crohn's) with a "not a reason to start smoking" caveat
- **Diverticular disease** — seventh condition added in Mia's version; not in the original six

---

## What was removed (intentionally)

| Element | Reason |
|---------|--------|
| Site header & nav | Widget is embedded in the WP hub — no double header |
| Dark teal hero banner | Replaced by clean `.cond-page-title` title strip |
| Footer | Not needed inside an iframe embed |
| "Related conditions" grid | Redundant — the left sidebar serves that function |
| `/widget/` variant | Merged into root — only one version now |
| `/mia/` variant | Promoted to root — this IS the default now |

---

## Embedding (iframe)

To embed on the WordPress hub:

```html
<iframe
  src="https://vance-gi-health.vercel.app"
  width="100%"
  height="800"
  frameborder="0"
  scrolling="auto"
  title="GI Health Information">
</iframe>
```

For a specific condition page (e.g. from a product page for a UC treatment):

```html
<iframe
  src="https://vance-gi-health.vercel.app/conditions/ulcerative-colitis.html"
  width="100%"
  height="900"
  frameborder="0"
  title="Ulcerative Colitis">
</iframe>
```

Consider adding `allow="fullscreen"` and a fixed `min-height` if embedding at narrow column widths.

---

## Adding a new condition

1. Create `conditions/<slug>.html` — copy an existing page as a template
2. Update the `<title>`, `<meta name="description">`, `h1`, and lede
3. Add the new condition's `<a>` to the `cond-nav` in **all** existing condition pages (and set `class="active"` on the new page itself)
4. Add a card to `index.html` in the `.grid-3` conditions section
5. Add the icon mask rule to `gi-health.css` under `/* per-condition icons */`
6. Commit and push

---

## Local development

```bash
# Python (no install needed)
python -m http.server 4321
# then open http://localhost:4321
```

Or use the VS Code Live Server extension pointed at the repo root.
