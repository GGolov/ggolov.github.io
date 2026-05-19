# Plan: Phase 1 — Visual Foundation & Homepage Redesign

**Mode:** mvp

## Overview

Apply the dark/neutral professional theme across the site and redesign the homepage as a single-column classic resume layout. This phase handles the global theme, header and footer redesign, and the homepage restructure — leaving the About page for Phase 2.

**Requirements covered:**
- VISUAL-01, VISUAL-02, VISUAL-03, VISUAL-04
- HOME-01, HOME-02, HOME-03, HOME-04, HOME-05
- HEADER-01, HEADER-02
- FOOTER-01, FOOTER-02

---

## Plan 1: Global Theme & Typography

**Requirements:** VISUAL-01, VISUAL-02, VISUAL-04

### Files to modify
| File | Action |
|------|--------|
| `src/styles/global.css` | Add CSS custom properties for dark theme, set serif headings, keep `* { transition-duration: 200ms; }` |

### Tasks

1. **Define CSS custom properties in `global.css`**

   Add a set of CSS custom properties on `:root` for the dark/neutral theme:
   - `--color-bg`: `#0f0f0f` — main background
   - `--color-surface`: `#16213e` — header/footer/card bg
   - `--color-text`: `#e8e8e8` — primary text
   - `--color-text-muted`: `#a0a0b0` — secondary text
   - `--color-accent`: `#4a90d9` — links, interactive elements
   - `--color-accent-hover`: `#6bb3f0` — hover states
   - `--color-border`: `#2a2a3e` — dividers

2. **Set font families via Tailwind's `@theme` directive**

   Use `@theme` in `global.css` to extend Tailwind's font families:
   - `font-family-serif`: system serif stack (`ui-serif, Georgia, Cambria, "Times New Roman", Times, serif`)
   - `font-family-sans`: system sans stack (default Tailwind stack)

3. **Apply base styles**

   Set `body` background to `var(--color-bg)`, text to `var(--color-text)`. Apply `font-sans` as default body font. Headings will use `font-serif` via utility classes in components.

### Verification
- `npm run build` succeeds without errors
- Body has dark background (`#0f0f0f`) with light text (`#e8e8e8`)
- No external fonts loaded (system fonts only)

---

## Plan 2: Header Redesign

**Requirements:** HEADER-01, HEADER-02, HOME-05

### Files to modify
| File | Action |
|------|--------|
| `src/components/Header.astro` | Redesign with dark theme, serif logo, nav with all links |

### Tasks

1. **Replace amber-400 color scheme** with dark surface bg (`#16213e` / `bg-[#16213e]`)
2. **Logo**: "GG" in serif font (`font-serif`), bold, ~36px (`text-4xl`), off-white text
3. **Navigation**: inline links for Home, Projects (and About for future Phase 2 — but current design keeps it simple: Home + Projects, About can be added in Phase 2)
4. **Active link**: underline with cool blue accent (`text-[#4a90d9]`)
5. **Spacing**: `justify-between`, `p-4` for padding, full height ~64px
6. **Social links** (from HOME-05): Add small icon links for LinkedIn and GitHub in the header area (right side, before nav or in nav)

### Verification
- Header renders with dark `#16213e` background
- "GG" logo is serif, large, and visible
- All nav links work and don't 404
- Social icons present and link to correct profiles

---

## Plan 3: Footer Redesign

**Requirements:** FOOTER-01, FOOTER-02

### Files to modify
| File | Action |
|------|--------|
| `src/components/Footer.astro` | Redesign with dark theme, remove Supabase |

### Tasks

1. **Replace amber-400 color scheme** with dark surface bg (`#16213e`)
2. **Remove Supabase** link and credit entirely
3. **Update copy** to: "Powered by Astro and hosted on GitHub Pages"
4. **Style**: right-aligned, muted text color (`text-[#a0a0b0]`), small text size (`text-sm`)
5. **Spacing**: `justify-end`, `p-4`

### Verification
- Footer renders with dark `#16213e` background
- No Supabase mention anywhere
- Text is right-aligned, small, muted color
- Links to Astro and GitHub Pages work

---

## Plan 4: Homepage Redesign — Single Column Resume

**Requirements:** HOME-01, HOME-02, HOME-03, HOME-04, VISUAL-03

### Files to modify
| File | Action |
|------|--------|
| `src/components/Home.astro` | Complete rewrite to single-column layout, fix HTML bug |

### Layout (top-to-bottom)

```
┌───────────────────────────────────────┐
│  Gleb Golov          (font-serif, 36px)│
│  Web Developer       (font-sans, 20px)│
│  ─────────────────── divider ────── │
│                                       │
│  Professional Summary / Bio           │
│  (placeholder text)                   │
│                                       │
│  ─────────────────── divider ────── │
│                                       │
│  ## Experience          (font-serif)  │
│                                       │
│  Web Developer                         │
│  Capgemini — Full Time                │
│  Oct 2022 - Present · Turin, Italy    │
│  (Hybrid)                             │
│                                       │
│  Q.A. Developer                        │
│  EpiCura — Full Time                  │
│  Oct 2022 - Present · Turin, Italy    │
│  (Hybrid)                             │
│                                       │
│  ─────────────────── divider ────── │
│                                       │
│  ## Contact                           │
│  [LinkedIn icon] LinkedIn             │
│  [GitHub icon] GitHub                 │
└───────────────────────────────────────┘
```

### Tasks

1. **Replace two-column layout** with single-column centered content (max-width ~720px, `mx-auto`)
2. **Add bio section** with placeholder professional summary
3. **Restructure experience list** to use valid HTML (fix the `<li>` inside `<li>` bug — each `<li>` must be a direct child of `<ol>`, and use a `<div>` or `<section>` inside for content)
4. **Format experience items** as: Job Title (bold/semibold, serif) → Company (sans, muted) → Date + Location (sans, small, muted)
5. **Move social links** from the old sidebar to a Contact section at the bottom
6. **Use proper dividers** between sections (`<hr class="border-[#2a2a3e]">` or similar)

### Verification
- Homepage renders as single column, centered, ~720px max-width
- No `<li>` inside `<li>` nesting — valid HTML
- Bio placeholder text visible at top
- Experience items in clean chronological format
- Social links at bottom with LinkedIn and GitHub icons
- Consistent dark/neutral styling

---

## Plan 5: Index Page & Layout Tidy

**Requirements:** VISUAL-03, VISUAL-04

### Files to modify
| File | Action |
|------|--------|
| `src/layouts/Layout.astro` | Minor — ensure body uses proper dark bg color |
| `src/pages/index.astro` | Minor — verify composition still works |

### Tasks

1. **Layout.astro**: Ensure `<body>` applies the dark background color and light text
2. **index.astro**: No changes expected, verify Header + Home + Footer composition renders correctly with new components
3. **Build check**: `npm run build` succeeds with no errors

### Verification
- Full page renders with dark background, light text throughout
- Header → Homepage content → Footer renders in correct order
- Build succeeds

---

## Risks

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| Breaking existing navigation | Low | Header component is self-contained; test all links after changes |
| Missing Tailwind color references | Low | Use inline hex values with Tailwind arbitrary syntax (`bg-[#16213e]`) instead of relying on theme config |
| HTML validation issues | Medium | The existing `<li>` nesting bug is explicitly fixed in Plan 4 |
| Supabase link removal creates broken layout | Low | Simple text replacement, no structural dependency |

## Dependencies

- None — Phase 1 is self-contained. No code depends on work from other phases.
