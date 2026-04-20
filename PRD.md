# PRD — dodeca Exhibition Catalog (Digital Booklet)

## 1. Overview

Transform the existing single-file `index.html` (currently "Lumière — The Autumn Edit", a luxury-furniture flipbook) into a digital exhibition catalog for **dodeca — 12 artists, one space**, an art show running **April 19–27, 2026** at **Gallery MOCD**.

The existing 3-leaf page-flip book is replaced with a 25-page flipbook whose content mirrors the supplied mockups in `WEBSITE/WEBSITE/1.jpg` … `25.jpg`. The interaction model (page-flip animation, desktop spread, mobile single-page deck, prev/next controls, keyboard + swipe) is retained; only the content, theme, and layout primitives change.

## 2. Goals

- Present 12 artists' works as a browsable, museum-quality catalog.
- Mirror the clean, editorial, cream-paper aesthetic of the mockups.
- Preserve the page-flip delight of the current booklet, but make it feel like an art publication, not a furniture look-book.
- Fully responsive: two-page spread on desktop, single-page deck on mobile/tablet.

## 3. Non-goals

- No CMS, back-end, or e-commerce.
- No inquiry/contact form (static catalog only).
- No i18n beyond preserving the mixed-script artwork titles (Hindi/Gujarati + English) already in the mockups.
- No audio, video, or zoom/lightbox viewer in v1 (can be v2).

## 4. Users

- **Visitors & collectors** browsing the exhibition ahead of or during the show.
- **Gallery staff** sharing a single link as a digital invite.
- **Artists** reviewing how their work is presented.

## 5. Design theme

Shift from dark-navy + gold to a **light, paper-forward gallery aesthetic**:

| Token          | Current (Lumière)     | New (dodeca)                          |
| -------------- | --------------------- | ------------------------------------- |
| Stage bg       | Deep navy radial      | Warm off-white (#f4f1ea → #ebe6d8)    |
| Page bg        | Cream + gold gradient | Near-white with subtle grid/graph texture |
| Ink            | Charcoal #2b2a26      | Near-black #1a1a1a                    |
| Accent         | Gold #b08a4a          | Rust / patina brown #7a4a2a (from MOCD logo) |
| Cover title    | Serif "Lumière"       | Serif "dodeca" (Didone/Bodoni feel)   |
| Body type      | Cormorant Garamond    | Serif for titles; wide-tracked sans for metadata |
| Metadata type  | Inter, tight          | Sans with **heavy letter-spacing** (~0.2em), matching mockups |
| Ornaments      | Gold diamonds/frames  | None — minimalism, whitespace-driven  |
| Page-edge shade| Heavy                 | Softened — paper, not leather         |

**Typography pairing:** `'Playfair Display'` or `'Cormorant Garamond'` (serif, for titles) + `'Inter'` or system sans with heavy tracking (for artist names, medium, dimensions, year).

**Cover texture:** faint grid (~22–24px graph lines at ~6% opacity) baked into the cover to match mockup 1.

## 6. Information architecture (pages)

Numbering below maps directly to `1.jpg` … `25.jpg`.

| # | Type       | Content                                                                 |
|---|------------|--------------------------------------------------------------------------|
| 1 | Cover      | **dodeca** · April 19–27 2026 · *12 artists, one space* · "Coming together in one space." |
| 2 | Artist     | Ashish Das — *From Purusha to Prakriti*, *Omnipotent Embrace* (bronze)   |
| 3 | Artist     | Ashish Das — *Reaching for the Moon*, *The Wind Within Her* (bronze)     |
| 4 | Artist     | Ashish Das — *Soaring high* (bronze) · Purvi Sharma — *Things you will see in my room 1* |
| 5 | Artist     | Purvi Sharma — *Things you will see in my room 2*, *My room-1*           |
| 6 | Artist     | Purvi Sharma — *My room-2*, *My room 3* · Parag Das — *Scrolls of Taste* |
| 7 | Artist     | Parag Das — *Scrolls of Taste* (two plates)                              |
| 8 | Artist     | Parag Das — *Scrolls of Taste* (two plates)                              |
| 9 | Artist     | Parag Das — *Scrolls of Taste* (two plates)                              |
| 10| Artist     | Kapil Jangid — *Bajwa* (two plates)                                      |
| 11| Artist     | Himanshu Jamod — *King boat*, *Retrive-2*                                |
| 12| Artist     | Vasudev M Nair — *The Fall I*, *The Fall II*, *Make a wish*              |
| 13| Artist     | Vasudev M Nair — *All Too Bright*, *Cloud of Unknowing*                  |
| 14| Artist     | Tanya Sharma — *Pathway…* (×2), *THE IDEA OF YOU* (ceramics)             |
| 15| Artist     | Tanya Sharma — *I see you* (ceramics, ×4)                                |
| 16| Artist     | Tanya Sharma — *I see you* (ceramics, ×4)                                |
| 17| Artist     | Tanya Sharma — *I see you* (ceramics, ×4)                                |
| 18| Artist     | Shiv Shankar — *The last tear*, *A strange spectacle*, *The Naked/Exposed Body* |
| 19| Artist     | Nagajan Karavadara — *Piro Mal*, *Kuvo*, *Mavthu*                        |
| 20| Artist     | Nagajan Karavadara — *Mavthu III*, *Mavthu II*                           |
| 21| Artist     | Dugaprasad Bandi — *Girl with her pet*, *Self in mask-3*, *Self in mask-2* |
| 22| Artist     | Dugaprasad Bandi — *Path leads to war & peace* (×2)                      |
| 23| Artist     | Digvijay Jadeja — *Collapse-2*, *Dream-2*, *Eidetic*, *Recall*           |
| 24| Artist     | Digvijay Jadeja — *Bohemian*, *Drenched*                                 |
| 25| Back cover | **GALLERY MOCD** logo                                                    |

Total leaves (front/back pairs) on desktop: **13** (page 1 front = cover; page 25 = back cover). On mobile, 25 single cards.

## 7. Page template

Every interior page uses a consistent grid:

- **Artwork image(s):** 1–4 images, positioned left/right/mixed per mockup.
- **Caption block** (right- or left-aligned to opposite the image):
  - Artist name (sans, tracked, regular)
  - *Title* (italic, slightly bolder)
  - Medium
  - Dimensions
  - Year
- Extreme letter-spacing (~0.18–0.22em) on caption text to match mockups.
- Generous whitespace; no borders, no ornaments.

## 8. Responsive behavior

- **≥ 980px (desktop spread):** two-page spread (reuse existing `.stage` / `.leaf` machinery). Image+caption layouts scale with `clamp()`.
- **621–979px (tablet):** single scaled-down spread OR switch to single-page deck — TBD during build; default to single-page for legibility of long captions.
- **≤ 620px (mobile):** existing `.mobile-stage` single-card deck. Multi-image pages reflow captions below each image (stack vertically).
- Image loading: `loading="lazy"` for pages ≥ 3; first 2 eager. Consider `srcset` once we have optimized assets.
- Honor `prefers-reduced-motion` (already in place — keep).

## 9. Technical constraints

- Stay single-file `index.html` (no build step) unless we split images out — images live in `WEBSITE/WEBSITE/*.jpg` and are referenced directly.
- Preserve existing flip JS; only extend the number of leaves and the content within each `.face`.
- No frameworks. Vanilla HTML/CSS/JS only.
- Keep existing keyboard (arrows, space) and swipe behavior.

## 10. Acceptance criteria

1. Opening `index.html` shows the dodeca cover (mockup 1) at desktop and mobile widths.
2. All 25 mockup pages are reachable via next/prev controls, keyboard, on-page hitzones, and swipe.
3. Page counter reads `00 / 24` through `24 / 24` (24 leaf flips between 25 pages) on desktop and `00 / 25` through `25 / 25` in mobile single-card mode.
4. Color palette, fonts, and letter-spacing visibly match the mockups on side-by-side comparison.
5. No horizontal scroll on any viewport from 320px → 1920px.
6. Lighthouse Accessibility ≥ 90 (buttons labeled, images have `alt` text with artist + title).
7. Flip animation still feels like paper — 1.5s cubic-bezier easing preserved.

## 11. Out-of-scope / v2 ideas

- Tap-to-zoom artwork lightbox.
- Artist bio pages between sections.
- Table of contents / jump-to-artist menu.
- Share links that open to a specific page (`#page=11`).
- Print stylesheet producing a PDF.
