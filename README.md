# Vance Health Hub — GI Health section (prototype)

A standalone static prototype of a **GI Health** section for `vancehealthhub.co.uk`, modelled on the
structure of Tillotts Pharma's GI-Health section but **rebranded to the Vance Health Hub look** and with
**all copy written fresh** as original patient-education content. No Tillotts text, product names or
adverse-event forms are reused.

It adds two conditions the UK Tillotts site omits: **Irritable Bowel Syndrome (IBS)** and
**Colorectal Cancer (CRC)**, for a current set of **6 conditions**.

## Contents

```
index.html                                  GI Health landing hub
conditions/
  inflammatory-bowel-disease.html
  ulcerative-colitis.html
  crohns-disease.html
  microscopic-colitis.html
  irritable-bowel-syndrome.html             (added)
  colorectal-cancer.html                    (added)
assets/
  css/gi-health.css                         design tokens + components + keyframes
  js/gi-health.js                           reveal-on-scroll, stat count-up, scrollspy, mobile nav
  img/  logo.png, favicon.png, icons/*.svg  copied from the live theme
```

## View it

It's a pure static site with relative paths, so just **double-click `index.html`** (or drag it into a
browser) — no build step or server required. If you prefer a local server and have Node or Python
installed, `npx serve .` or `python -m http.server` also work.

## Design

Tokens mirror `sla-health-hub/assets/css/main.css` exactly so markup ports cleanly:
teal `#008080`, navy `#0A1929`, **square corners** everywhere, **Outfit** headings + **Inter** body,
1200px container, 80px sticky header, `.section-padding` / `.container` / `.btn-primary` / `.content-card`
/ `.grid-3` reused verbatim.

## Animations

All vanilla CSS + ~120 lines of JS, and all disabled under `prefers-reduced-motion`:

- Hero GI-tract SVG "draws in" (`stroke-dashoffset`) with floating blur orbs.
- Reveal-on-scroll fade/slide (`.reveal` → `.is-visible`) via one IntersectionObserver, staggered with `--reveal-delay`.
- Per-condition anatomy SVG with a pulsing highlighted region.
- Stat / key-fact numbers count up when scrolled into view (`data-count`).
- Card hover lift; sticky sidebar scrollspy highlights the active in-page section.

## Porting to the live WordPress theme (`sla-health-hub`)

The prototype is built to drop into the classic PHP theme in the sibling repo `VANCE-HealthHub-WP`.

1. **Pages, not a CPT.** Create a parent WP Page **GI Health** (`/gi-health/`) and one child Page per
   condition (`/gi-health/ulcerative-colitis/`, `/gi-health/irritable-bowel-syndrome/`, …). Hierarchical
   permalinks reproduce the Tillotts URL structure. Paste each file's main markup into the page editor
   (or a page template) — the body between `<section class="cond-hero">` and the end of the related-
   conditions section.
3. **Header / footer** collapse into the theme's `header.php` / `footer.php`; drop the per-file copies.
4. **CSS** — move `gi-health.css` rules into `assets/css/main.css` (or `wp_enqueue_style` it as a section
   sheet) and **bump the enqueue version** in `functions.php` after deploy so caches refresh.
5. **JS** — enqueue `gi-health.js` with `wp_enqueue_script`.
6. **Assets** — `assets/img/` references map to `get_template_directory_uri() . '/assets/img/...'`.
7. **Menu** — add a **GI Health** item to the Primary Menu with the 6 conditions as sub-items
   (Appearance → Menus).
8. **Deploy** via the existing Hostinger SSH tar/extract process documented in `VANCE-HealthHub-WP/CLAUDE.md`,
   then purge Hostinger + LiteSpeed caches.

### Load-bearing constraints (from `VANCE-HealthHub-WP/CLAUDE.md`)
- Do **not** rename the `sla-health-hub` theme folder or the `_sla_*` user-meta keys.
- Never run a bare `SLA` → `Vance` search-replace.

## Content note

Pages are clinician-style summaries written for this prototype and cite public UK sources
(NHS, NICE, Guts UK, Crohn's & Colitis UK, Bowel Cancer UK, Cancer Research UK). They are for
information only and should be reviewed/approved before publication.
