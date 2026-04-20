# Execution Plan — dodeca Catalog

Target: rewrite `D:/booklet/index.html` in place to deliver the PRD. Assets live in `D:/booklet/WEBSITE/WEBSITE/1.jpg … 25.jpg`.

## Phase 0 — Prep (15 min)

1. **Back up** current booklet: copy `index.html` → `index.lumiere.backup.html`.
2. **Audit images:** confirm all 25 JPGs open, note which are "whole-page mockups" vs "source artwork photos embedded in a page." *Decision:* treat each JPG as a ready-made page rendering; render pages as `<img>` first (fastest path to parity), then optionally rebuild as native HTML later (Phase 4).
3. Load fonts: add `<link>` for **Playfair Display** (serif) and **Inter** (sans) from Google Fonts with `display=swap`.

## Phase 1 — Theme swap (30 min)

Edit the `:root` block and a handful of gradients. No DOM changes.

1. Replace CSS variables:
   ```css
   --paper:    #f6f2e8;   /* page bg */
   --paper-2:  #ecE6d4;   /* page edge */
   --stage-1:  #efe9db;   /* stage bg top */
   --stage-2:  #d9cfb6;   /* stage bg bottom */
   --ink:      #1a1a1a;
   --ink-soft: #4a4a4a;
   --accent:   #7a4a2a;   /* MOCD rust */
   --cover:    #f6f2e8;   /* cover uses paper, not leather */
   ```
2. Rewrite `body` gradient to light warm off-white.
3. Soften `.stage::after` shadow (light scene → less drop-shadow).
4. Replace `.face` cream gradient with near-white + grid texture.
5. Swap `.cover` background to paper (not leather green).
6. Remove gold borders (`.cover-frame`) — mockups have no borders.
7. Update dotted paper-noise overlay to be paler on light bg.
8. Update header brand text → `DODECA` + tag → `GALLERY MOCD · April 19–27, 2026`.

**Gate:** open the page; confirm overall temperature is light/cream, not dark navy.

## Phase 2 — Page scaffold (45 min)

Replace the three existing `.leaf` blocks with a data-driven generator.

1. Define a `PAGES` array in JS:
   ```js
   const PAGES = [
     { src: 'WEBSITE/WEBSITE/1.jpg',  alt: 'Cover — dodeca, 12 artists one space' },
     { src: 'WEBSITE/WEBSITE/2.jpg',  alt: 'Ashish Das — Purusha to Prakriti, Omnipotent Embrace' },
     …
     { src: 'WEBSITE/WEBSITE/25.jpg', alt: 'Gallery MOCD' },
   ];
   ```
2. On load, loop `PAGES` in pairs and inject `<div class="leaf">…</div>` with front/back faces containing `<img class="page-img" loading="lazy" src alt>`.
3. Odd page count (25) → final leaf has a blank back (or reuse the MOCD logo).
4. Rebuild mobile deck from the same array (each page → one `.m-leaf`).
5. Remove all Lumière product SVGs, divider SVGs, and copy.
6. Update `.face` CSS: image fills container with `object-fit: contain`, white bg behind.

**Gate:** all 25 pages are reachable via next/prev; counter reads `00 / 24` (desktop, 24 flips) / `00 / 25` (mobile).

## Phase 3 — Responsive polish (45 min)

1. **Desktop ≥ 980px:** two-page spread at `16/10` aspect — current code works; verify images crop cleanly.
2. **Tablet 621–979px:** force single-card mode (reuse mobile deck). Add `@media (max-width: 979px) { .stage { display: none; } .mobile-stage { display: block; } }`.
3. **Mobile ≤ 620px:** already single-card; tune card aspect to `3/4` and clamp controls.
4. **Controls:** enlarge tap targets on touch (`min-height: 44px`), lighten border since bg is light now.
5. **Hit zones:** keep 28% left/right for desktop, 40% for mobile.
6. Verify no horizontal scroll at 320, 375, 414, 768, 1024, 1440, 1920.
7. Run Lighthouse → fix any a11y complaints (button labels already good; add `alt` on imgs).

**Gate:** resize browser through breakpoints; flip, swipe, keyboard all work.

## Phase 4 — (Optional) Native text rendering (2–3 hrs)

Only if the JPG-as-page approach looks pixelated on retina or captions aren't crisp. For each interior mockup:

1. Crop/export artwork images separately to `assets/art/`.
2. Rebuild page as HTML: absolute-positioned `<img>` + caption `<figcaption>` with the right font/tracking.
3. Keep mockup JPG as visual reference while tuning.

Skip this phase for v1 if the JPGs look sharp — ship faster.

## Phase 5 — QA & ship (30 min)

1. Cross-browser: latest Chrome, Firefox, Safari (desktop + iOS).
2. Keyboard: arrows + space navigate; no focus traps.
3. Swipe on touch: left/right advances by one page.
4. `prefers-reduced-motion`: animation duration drops to 0.4s (already coded).
5. Open in an incognito window on a fresh profile (no font cache) — confirm FOUT is graceful.
6. Replace `index.html` on whatever host serves it (file-system double-click works fine for local review).

## Risks & mitigations

| Risk | Mitigation |
|---|---|
| Mockup JPGs are portrait (A4-ish), desktop spread is 16:10 landscape. Pages will letterbox. | Use `object-fit: contain` with paper-colored padding, OR switch desktop aspect to `7/5` to better match A4 pairs. |
| 25 JPGs = ~heavy first load | `loading="lazy"` on pages ≥ 3; if still heavy, generate WebP siblings (~60% smaller) in Phase 4. |
| Flip code assumes even page count | Add a blank back face to leaf 13, or treat page 25 as a solo right-page after leaf 12. Prefer the blank-back approach — minimal code churn. |
| Fonts FOUT on slow networks | `font-display: swap` + fallback `Georgia, 'Times New Roman'` for serif; `system-ui` for sans. |

## Timeline

- Total: **~2.5 hours** for Phases 0–3 (ship-ready v1).
- Add **~2–3 hours** for Phase 4 if native-text rebuild is needed.

## Go/no-go checklist before ship

- [ ] Cover matches mockup 1 at desktop and mobile widths.
- [ ] All 25 pages reachable via 3 input methods (buttons, keys, swipe).
- [ ] Page counter accurate on both desktop and mobile.
- [ ] No console errors, no 404s on images.
- [ ] No horizontal scroll at 320–1920px.
- [ ] Lighthouse a11y ≥ 90.
- [ ] `prefers-reduced-motion` respected.
