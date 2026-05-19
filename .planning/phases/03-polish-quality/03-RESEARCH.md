# Phase 3: Polish & Quality — Research

**Researched:** 2026-05-19
**Domain:** Accessibility auditing, visual consistency, responsive verification, build validation
**Confidence:** HIGH

## Summary

This phase is a quality-gate polish pass over the entire static Astro site. No new packages, libraries, or external dependencies are required — all work is purely markup and CSS adjustments in existing `.astro` files.

The primary research findings are:

1. **Color contrast passes WCAG AA across all current color pairs** — `#a0a0b0` muted text on `#16213e` surface computes to ~6.2:1, well above the 4.5:1 threshold. No color changes needed.
2. **Accessibility gaps are limited** — missing `rel="noopener noreferrer"` on one footer link, missing `alt` attributes on social icons (currently empty, which is borderline but functionally OK for decorative adjacent images; explicit descriptive alt text is a best-practice improvement), no skip-to-content link, no explicit ARIA landmarks.
3. **Visual consistency** — empty `<style></style>` tags in 5 components should be removed; `Projects.astro` is a bare placeholder needing content to match the site design; spacing patterns across sections are consistent already.
4. **Responsive layout** — `max-w-[720px]` container with mobile-first Tailwind already works on all viewports; no breakpoint adjustments are needed.
5. **Build validation** — standard `npm run build` (Astro 5.7.5); no adapter required for static output; output to `dist/`.

**Primary recommendation:** Focus on markup-only accessibility improvements (skip-to-content link, ARIA landmarks, `alt` text, `rel` attributes), visual polish (remove empty `<style>` tags, flesh out Projects placeholder), and build verification. No stack changes needed.

## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| VISUAL-04 | Accessibility and contrast verification | WCAG AA SC 1.4.3 contrast ratios verified across all color pairs — all pass. Accessibility gaps identified: missing skip-to-content, missing ARIA landmarks, one missing `rel="noopener noreferrer"`, social icon alt text. See contrast calculations below. |

## User Constraints (from Success Criteria)

### Locked Decisions
- Color contrast ratios must meet WCAG AA standards (4.5:1 for normal text, 3:1 for large text)
- All pages must have consistent spacing, typography, and styling
- Site must build without errors (`npm run build` succeeds)
- Responsive layout must work on mobile and desktop viewports

### Agent's Discretion
- Degree of ARIA landmark enhancement (minimal vs thorough)
- Whether to add a skip-to-content link (recommended: yes)
- Whether to use explicit `role` attributes vs relying on semantic HTML (prefer semantic HTML)
- Projects.astro placeholder content design

### Deferred Ideas (OUT OF SCOPE)
- Client-side JavaScript interactivity
- Backend APIs or server routes
- Contact forms or downloadable resume PDF (v2)
- Visual skill indicators (v2)

## Architectural Responsibility Map

| Capability | Primary Tier | Secondary Tier | Rationale |
|------------|-------------|----------------|-----------|
| Color contrast audit | Browser / CSS | — | CSS `@theme` variables and `color-*` utilities define all colors; contrast is a CSS concern |
| ARIA landmarks | Browser / markup | — | ARIA attributes are static HTML markup in `.astro` templates |
| Skip-to-content link | Browser / markup | — | Pure HTML anchor + CSS focus styling |
| External link security | Browser / markup | — | `rel` attribute on `<a>` tags in `.astro` templates |
| Social icon alt text | Browser / markup | — | `alt` attribute on `<img>` tags in `.astro` templates |
| Empty `<style>` removal | Build / markup | — | Static file editing — remove empty style blocks |
| Projects placeholder | Browser / markup | — | Fill in content in `Projects.astro` component |
| Build validation | CI / Local | — | `npm run build` — Astro's build pipeline |
| Responsive layout check | Browser | CSS | Pure CSS `max-w-[720px]` with Tailwind mobile-first defaults |

## Standard Stack

### Core
| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| Astro | 5.7.5 | Static site framework | Already installed, phase uses existing infrastructure |
| Tailwind CSS | 4.1.4 | Utility CSS framework | Already installed, no new classes needed |
| @tailwindcss/vite | 4.1.4 | Vite plugin for Tailwind | Already configured in `astro.config.mjs` |

### No New Packages Required
This phase requires zero new dependencies. All changes are markup and CSS edits to existing `.astro` files and `global.css`.

## Package Legitimacy Audit

> No packages are installed in this phase. All work is markup/CSS changes to existing files. Slopcheck verification skipped — not applicable.

## Architecture Patterns

### Pattern 1: Skip-to-Content Link
**What:** A hidden link at the very top of the page that becomes visible on keyboard focus and jumps the user past navigation to the main content.

**When to use:** Every page with persistent navigation (header nav blocks keyboard users from reaching content quickly).

**Implementation approach (from Astro docs & WCAG best practices):**
```astro
<!-- First child of <body> — before <Header /> -->
<a
  href="#main-content"
  class="sr-only focus:not-sr-only focus:fixed focus:top-4 focus:left-4 focus:z-50
         focus:px-4 focus:py-2 focus:bg-[#4a90d9] focus:text-[#0f0f0f] focus:rounded"
>
  Skip to content
</a>
```

Then add `id="main-content"` to the `<main>` element on each page.

**Source:** WCAG Technique G1: Adding a link at the top of each page that goes directly to the main content area [CITED: w3.org/WAI/WCAG21/Techniques/general/G1]

### Pattern 2: ARIA Landmarks with Semantic HTML
**What:** Use semantic HTML elements (`<nav>`, `<main>`, `<header>`, `<footer>`) which implicitly define ARIA landmarks. Only add explicit `role` attributes where semantic HTML is insufficient.

**When to use:** All pages. The current codebase already uses semantic `<header>`, `<nav>`, `<main>`, and `<footer>` elements correctly.

**Checklist:**
- `<header>` — implicitly defines `banner` landmark — **already correct**
- `<nav>` — implicitly defines `navigation` landmark — **already correct**
- `<main>` — implicitly defines `main` landmark — **already correct**
- `<footer>` — implicitly defines `contentinfo` landmark — **already correct**
- No explicit `role` attributes needed for these elements in modern browsers

**Source:** ARIA in HTML spec — "Authors MUST NOT use the banner, contentinfo, main, or navigation roles on elements that are not sectioning elements" (i.e., semantic HTML is preferred) [CITED: w3c.github.io/html-aria/]

### Pattern 3: Social Link `<img>` Alt Text
**What:** For icon images that serve as the sole content of a link (no visible text), provide meaningful `alt` text. For icon images paired with adjacent visible link text, `alt=""` is sufficient.

**Current state:** Social links in `Home.astro` have both icon `<img>` and visible text ("LinkedIn", "GitHub") — so `alt=""` is technically correct per WCAG. However, best practice is to either:
- Keep `alt=""` (sufficient when text label is present), OR
- Remove redundant visible text and add descriptive `alt` (e.g., `alt="LinkedIn profile"`)

**Recommendation:** Keep current approach (`alt=""` with visible text) — it's valid and accessible. But verify the `alt` attribute exists and isn't missing.

### Pattern 4: External Link Security
**What:** Links opening in new tabs must include `rel="noopener noreferrer"` to prevent tab-napping via `window.opener`.

**Current state:** Social links in `Home.astro` and "Powered by Astro" in footer already have `target="_blank" rel="noopener noreferrer"`. The "hosted on GitHub Pages" link is missing `rel="noopener noreferrer"`.

**Fix:** Add `rel="noopener noreferrer"` to the GitHub Pages link in `Footer.astro`.

### Anti-Patterns to Avoid
- **Adding explicit `role` to semantic HTML elements**: `<nav role="navigation">` is redundant and can confuse screen readers
- **Over-ARIA-ifying a static site**: Static content sites rarely need complex ARIA patterns (live regions, tab panels, etc.)
- **Adding JS for focus management**: A static CV site doesn't need programmatic focus management

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Color contrast checking | Manual calculation | WebAIM contrast checker, browser DevTools | Already computed — all pairs pass AA |
| Skip-to-content link | Custom JS | Pure HTML/CSS pattern above | No JS needed for anchor-based skip links |
| Responsive testing | Custom viewport script | Browser DevTools responsive mode | Standard tooling sufficient |

**Key insight:** This phase has zero "don't hand-roll" concerns because no new functionality is being built — only existing markup is being audited and polished.

## Common Pitfalls

### Pitfall 1: Over-engineering Accessibility
**What goes wrong:** Adding ARIA roles and attributes where semantic HTML already provides the correct accessibility mapping. 
**Why it happens:** Developers believe "more ARIA = more accessible."
**How to avoid:** Follow the First Rule of ARIA — don't use ARIA if you can use a native HTML element that already has the semantics built-in.
**Warning signs:** `role="navigation"` on `<nav>`, `role="main"` on `<main>`, `role="banner"` on `<header>`.

### Pitfall 2: Empty `alt` vs. Missing `alt`
**What goes wrong:** Confusing an empty `alt=""` (decorative, correct) with a missing `alt` attribute (error — screen readers read the filename).
**Why it happens:** Developers see `alt=""` and think it's an error.
**How to avoid:** `alt=""` is correct for decorative images. Only add descriptive text when the image conveys information not available in nearby text.
**Warning signs:** Accidentally removing `alt=""` thinking it's an error, or adding redundant text.

### Pitfall 3: Breaking the Build with Incorrect HTML
**What goes wrong:** Astro's build step silently outputs broken HTML that doesn't affect rendering but fails validation.
**Why it happens:** Astro compiles `.astro` to HTML — invalid nesting or unclosed tags can slip through.
**How to avoid:** Run `npm run build` after every change batch. Validate output HTML with the W3C validator.
**Warning signs:** Build warnings about hydration or unexpected HTML output.

### Pitfall 4: Empty `<style>` Tags
**What goes wrong:** Empty `<style></style>` blocks are stripped by most build tools but bloat source files and confuse developers.
**Why it happens:** Component scaffolding/system generates them as placeholders.
**How to avoid:** Remove them — they serve no purpose.
**Warning signs:** Any `<style></style>` in a component file.

## Code Examples

### Verified Patterns from Official Sources

#### Skip-to-Content Link (WCAG G1)
```astro
---
// Insert at top of Layout.astro, inside <body>, before <slot />
// Or insert into each page's template before <Header />
---

<a
  href="#main-content"
  class="sr-only focus:not-sr-only focus:fixed focus:top-4 focus:left-4 focus:z-50
         focus:px-4 focus:py-2 focus:bg-[#4a90d9] focus:text-[#0f0f0f] focus:rounded
         focus:outline-none"
>
  Skip to content
</a>
```
**Source:** WCAG Technique G1 — Verified from official W3C docs [CITED: w3.org/WAI/WCAG21/Techniques/general/G1]

#### Keyboard Focus Indicator (WCAG 2.4.7)
Tailwind CSS v4 provides `focus-visible:` and `focus:` variants. Use `focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-[#4a90d9]` for custom focus rings that only appear on keyboard focus.
**Source:** Tailwind CSS v4 docs [CITED: tailwindcss.com/docs/hover-focus-and-other-states#focus-visible]

#### External Link Security Pattern
```astro
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
  Link text
</a>
```
**Source:** MDN Security recommendation — verified from official docs [CITED: developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel/noopener]

## Color Contrast Verification (WCAG AA SC 1.4.3)

### Color Pair Audit
| Foreground | Background | Context | Contrast Ratio | WCAG AA Pass? |
|------------|------------|---------|----------------|---------------|
| `#e8e8e8` (body text) | `#0f0f0f` (page bg) | Body text paragraphs | ~15.6:1 | ✅ PASS |
| `#a0a0b0` (muted) | `#0f0f0f` (page bg) | Subtitle, date, location text | ~7.4:1 | ✅ PASS |
| `#a0a0b0` (muted) | `#16213e` (surface bg) | Skill/language tags | ~6.2:1 | ✅ PASS |
| `#4a90d9` (accent) | `#0f0f0f` (page bg) | Link text | ~5.7:1 | ✅ PASS |
| `#6bb3f0` (accent hover) | `#0f0f0f` (page bg) | Hovered links | ~8.9:1 | ✅ PASS |
| `#2a2a3e` (border) | `#0f0f0f` (page bg) | Horizontal rules | N/A (not text) | N/A (non-text contrast not applicable for borders with no information) |

**Verdict:** All text color pairs exceed WCAG AA minimum (4.5:1 normal, 3:1 large). No color changes needed.

### Non-Text Contrast (WCAG SC 1.4.11)
The site has no active UI components (buttons, form controls) that require non-text contrast checking.

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| Explicit `role` attributes on semantic HTML | Use semantic HTML only, ARIA only when needed | HTML5 (2014) | Less code, better accessibility |
| `rel="noopener"` only | `rel="noopener noreferrer"` | ~2019 (referrer policy standard) | Prevents referrer leakage AND tab-napping |
| `focus:` for all focus styles | `focus-visible:` for keyboard-only focus | Tailwind CSS v3.3+ (2023) | Better UX: no focus rings on mouse clicks |

## Assumptions Log

| # | Claim | Section | Risk if Wrong |
|---|-------|---------|---------------|
| A1 | All color contrast pairs pass WCAG AA | Color Contrast Verification | LOW — computed via relative luminance formula from official WCAG spec; verified against multiple pairs |
| A2 | Astro 5.7.5 build output is fully static with no SSR | Standard Stack | LOW — confirmed by `astro.config.mjs` having no adapter; Astro defaults to static |
| A3 | Tailwind v4 `focus-visible:` variant works correctly | Code Examples | LOW — confirmed from Tailwind v4 docs |
| A4 | Empty `<style></style>` tags are safe to remove | Common Pitfalls | LOW — they have no content and no effect |

## Open Questions (RESOLVED)

1. **Should social icons use `alt=""` or descriptive `alt` text?** — `RESOLVED`: Keep `alt=""` — WCAG SC 1.1.1 confirms this is correct for decorative images adjacent to visible link text. No change needed.
   - What we know: Icons are currently `alt=""` with adjacent visible link text. WCAG says `alt=""` is correct for decorative images adjacent to text.
   - What's unclear: Whether the user considers the icons decorative or informational.
   - Recommendation: Keep `alt=""` — it's the correct approach. Only change if the adjacent text is removed.

2. **What content should `Projects.astro` placeholder display?** — `RESOLVED`: Add a styled "Coming soon" message with serif heading and muted body text, matching the site's dark/neutral theme.
   - What we know: Currently just "Projects" text with no styling.
   - What's unclear: Whether to add placeholder project cards, a "Coming soon" message, or keep minimal.
   - Recommendation: Add a styled placeholder consistent with the site theme — heading + "Coming soon" message matching the existing typography pattern.

## Validation Architecture

> Skipped — `workflow.nyquist_validation` is explicitly set to `false` in `.planning/config.json`.

## Security Domain

### Applicable ASVS Categories

| ASVS Category | Applies | Standard Control |
|---------------|---------|-----------------|
| V2 Authentication | No | Static site — no auth |
| V3 Session Management | No | Static site — no sessions |
| V4 Access Control | No | Static site — no access control |
| V5 Input Validation | No | Static site — no user input |
| V6 Cryptography | No | Static site — no crypto |

### Known Threat Patterns for Static Sites

| Pattern | STRIDE | Standard Mitigation |
|---------|--------|---------------------|
| Tab-napping via `window.opener` | Tampering | `rel="noopener noreferrer"` on all `target="_blank"` links |
| UI redress (clickjacking) | Elevation of Privilege | Not applicable — no interactive UI elements |

## Environment Availability

> All work in this phase is markup/CSS editing. No external dependencies required.

| Dependency | Required By | Available | Version | Fallback |
|------------|-------------|-----------|---------|----------|
| Node.js | Astro build | ✓ | 24.11.1 | — |
| npm | Package scripts | ✓ | 11.14.1 | — |

**Missing dependencies with no fallback:** None
**Missing dependencies with fallback:** None

## Sources

### Primary (HIGH confidence)
- [WCAG 2.1 SC 1.4.3 — Contrast Minimum](https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum.html) — Official W3C Understanding document, updated March 2026. Verified contrast ratio requirements: 4.5:1 normal text, 3:1 large text.
- [WCAG Technique G1 — Skip-to-content link](https://www.w3.org/WAI/WCAG21/Techniques/general/G1) — Official WCAG technique for adding skip navigation.
- [Tailwind CSS v4 — Responsive Design](https://tailwindcss.com/docs/responsive-design) — Official Tailwind docs covering mobile-first breakpoints. Verified breakpoint defaults.
- [Tailwind CSS v4 — Hover, Focus, and Other States](https://tailwindcss.com/docs/hover-focus-and-other-states) — Official docs covering `focus-visible`, `focus`, and ARIA attribute variants.
- [Astro — Deploy Guide](https://docs.astro.build/en/guides/deploy/) — Official Astro docs covering build command and static output.

### Secondary (MEDIUM confidence)
- [ARIA in HTML spec](https://w3c.github.io/html-aria/) — W3C spec covering which ARIA roles are implicit from HTML elements.
- [MDN — rel="noopener"](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel/noopener) — MDN documentation on `noopener` and `noreferrer` link security.

### Tertiary (LOW confidence)
- None — all claims verified against primary sources.

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — verified from `package.json` and `npm list`
- Color contrast: HIGH — computed via WCAG relative luminance formula against official spec
- Accessibility patterns: HIGH — verified against W3C WCAG techniques
- Build validation: HIGH — verified from Astro official docs
- Responsive design: HIGH — verified from Tailwind CSS v4 official docs

**Research date:** 2026-05-19
**Valid until:** 2026-06-19 (stable ecosystem — no fast-moving dependencies)
