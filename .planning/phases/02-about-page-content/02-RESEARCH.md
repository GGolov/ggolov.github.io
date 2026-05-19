# Phase 2: About Page & Content — Research

**Researched:** 2026-05-19
**Domain:** Static page composition, Astro routing, CV content layout
**Confidence:** HIGH

## Summary

Phase 2 creates a new `/about` route with bio, skills, and education sections — all static markup with no new dependencies. The work maps cleanly to 4 plans: (1) create the About page route + component with all sections, (2) create placeholder tech categories for skills, (3) add the About link to the header navigation, (4) integrate the profile photo placeholder.

The established patterns from Phase 1 (single-column centered layout, dark/neutral theme tokens, section dividers, serif headings / sans-serif body) are fully reusable. No new packages, build tools, or third-party services are required.

**Primary recommendation:** Create `src/pages/about.astro` (page route) + `src/components/About.astro` (section component), following the exact same composition pattern as `projects/index.astro`.

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| ABOUT-01 | Create `/about` page with professional bio section | Astro file-based routing: `src/pages/about.astro` → `/about`. Bio section uses existing `Home.astro` layout pattern. |
| ABOUT-02 | Add skills/tech stack section with categorized technologies | Skills section uses tag-style items per D-07, D-08, D-09. Categories: Frontend, Backend, Tools & DevOps. |
| ABOUT-03 | Add education section with academic background | Simple format per D-12: degree, institution, year. Most recent first per D-13. |
| ABOUT-04 | Fix the About link in header navigation | Header.astro `<nav>` currently has only Home and Projects — missing About `<a>` tag. |
</phase_requirements>

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions
- **D-01:** Sections appear top-to-bottom as: Bio → Skills → Education
- **D-02:** Include a profile photo/avatar at the top of the page (user will provide the file path during planning)
- **D-03:** Include location (Turin, Italy) after the bio — shown in text form, not as a separate section
- **D-04:** Include a languages section — list of languages spoken (e.g., English, Italian, Russian) as a compact list or tags
- **D-05:** Single-column centered layout (~720px max-width), matching the homepage design from Phase 1
- **D-06:** No contact section on the About page — the Contact section on the homepage is sufficient
- **D-07:** Skills organized by category (Frontend, Backend, Tools & DevOps) — each category as a subheading with tag-style items underneath
- **D-08:** Use standard web development categories and technologies as placeholder content for now — user can refine the exact list later
- **D-09:** No proficiency levels or visual indicators — simple tag-style listing
- **D-10:** Professional CV summary — concise, formal, 2-3 paragraphs
- **D-11:** Similar tone to LinkedIn summary — covers role, expertise, and professional approach
- **D-12:** Education — simple format: degree, institution, year per entry. Clean and scannable, no descriptions or achievements.
- **D-13:** Education in chronological order (most recent first)
- **D-14:** User will provide the photo file path during planning — add a placeholder or configurable image slot

### The agent's Discretion
No discretion items — all decisions are locked.

### Deferred Ideas (OUT OF SCOPE)
None — discussion stayed within phase scope.
</user_constraints>

## Architectural Responsibility Map

| Capability | Primary Tier | Secondary Tier | Rationale |
|------------|-------------|----------------|-----------|
| Page routing | Astro SSG | — | Astro file-based routing maps `src/pages/about.astro` to `/about` |
| Page composition | Astro SSG | — | Layout wraps Header + About + Footer — same pattern as existing pages |
| Bio content | Astro Component | — | Static HTML/Tailwind in `About.astro` — no server or client logic |
| Skills display | Astro Component | — | Static tag-style HTML in `About.astro` — no interactivity needed |
| Education display | Astro Component | — | Static list in `About.astro` — most-recent-first ordering |
| Navigation link | Browser (Header) | — | Static `<a>` tag in `Header.astro` pointing to `/about` |
| Profile photo | CDN / Static | Astro Assets | Photo lives in `src/assets/` and is imported as static asset |

## Standard Stack

No new packages are required for this phase. All work uses the existing stack:

| Library | Version | Purpose |
|---------|---------|---------|
| Astro | ^5.7.5 | Static site generation, file-based routing, component composition |
| Tailwind CSS | ^4.1.4 | Utility-first styling — all visual design via Tailwind classes |

**No installation required.** This phase is pure markup/content.

## Package Legitimacy Audit

> No external packages are installed in this phase. Skipping Package Legitimacy Gate — not applicable.

## Architecture Patterns

### System Architecture Diagram

```
┌─────────────────────────────────────┐
│          Browser (HTTP)              │
└──────────┬──────────────────────────┘
           │ GET /about
           ▼
┌─────────────────────────────────────┐
│      Astro SSG (build time)          │
│                                      │
│  src/pages/about.astro               │
│    ┌─────────────────────────────┐   │
│    │ Layout.astro (shell)        │   │
│    │  ├── Header.astro           │   │
│    │  │   └── nav: /about link   │   │
│    │  ├── About.astro (content)  │   │
│    │  │   ├── Profile photo      │   │
│    │  │   ├── Bio section        │   │
│    │  │   │   └── Location       │   │
│    │  │   │   └── Languages      │   │
│    │  │   ├── Skills section     │   │
│    │  │   │   ├── Frontend tags  │   │
│    │  │   │   ├── Backend tags   │   │
│    │  │   │   └── Tools/DevOps   │   │
│    │  │   └── Education section  │   │
│    │  └── Footer.astro           │   │
│    └─────────────────────────────┘   │
└─────────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────┐
│       Static Output (dist/)          │
│  → /about/index.html                 │
└─────────────────────────────────────┘
```

### Recommended Project Structure Impact

This phase adds exactly 2 files and modifies 1 file:

```
src/
├── pages/
│   └── about.astro          ← NEW — page route for /about
├── components/
│   └── About.astro          ← NEW — main content component
└── components/
    └── Header.astro         ← EDIT — add About nav link
```

### Pattern 1: Page Composition (follows existing pattern)
**What:** Compose a page from Layout + Header + component + Footer.
**When to use:** Every new page route.
**Example (from `projects/index.astro`):**
```astro
---
import Layout from "../../layouts/Layout.astro";
import Header from "../../components/Header.astro";
import Footer from "../../components/Footer.astro";
import About from "../../components/About.astro";
---

<Layout>
  <Header />
  <About />
  <Footer />
</Layout>
```

### Pattern 2: Section Layout with Dividers (follows `Home.astro`)
**What:** Sections separated by `<hr>` dividers, each with a serif heading.
**When to use:** Any multi-section page component.
**Example (from `Home.astro`):**
```astro
<section class="mb-6">
  <h2 class="font-serif font-bold text-2xl text-[#e8e8e8] mb-4">Section Title</h2>
  <!-- content -->
</section>

<hr class="border-[#2a2a3e] mb-6" />
```

### Pattern 3: Tag-style Skill Items
**What:** Small rounded containers for technology tags, grouped by category subheading.
**When to use:** Skills section.
```astro
<h3 class="font-serif font-semibold text-lg text-[#e8e8e8] mb-3">Frontend</h3>
<div class="flex flex-wrap gap-2 mb-6">
  <span class="px-3 py-1 text-sm bg-[#16213e] text-[#a0a0b0] rounded-full">React</span>
  <span class="px-3 py-1 text-sm bg-[#16213e] text-[#a0a0b0] rounded-full">TypeScript</span>
  <!-- more tags -->
</div>
```

### Pattern 4: Education List Entry
**What:** Simple scannable format — degree, institution, year.
```astro
<div class="mb-4">
  <h3 class="font-serif font-semibold text-lg text-[#e8e8e8]">Degree Name</h3>
  <p class="font-sans text-sm text-[#a0a0b0]">Institution Name</p>
  <p class="font-sans text-sm text-[#a0a0b0]">Year</p>
</div>
```

### Anti-Patterns to Avoid
- **Adding a contact section:** D-06 explicitly says no contact section on About page.
- **Using proficiency bars/indicators:** D-09 says no proficiency levels.
- **Adding client-side JS:** This is a fully static page — no scripts needed.
- **Nesting `<li>` inside `<li>`:** Previous bug in `Home.astro` — use sibling `<li>` elements.

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Page routing | Custom router | Astro file-based routing | Built into Astro — no config needed |
| CSS framework | Custom CSS system | Tailwind utility classes | Already established in Phase 1 |
| Image optimization | Custom image pipeline | Astro `<Image />` or imported `<img>` | Built into Astro 5.x via `astro:assets` |

**Key insight:** This phase needs zero custom infrastructure — every requirement maps to an existing Astro/Tailwind pattern already present in the codebase.

## Common Pitfalls

### Pitfall 1: Missing About Link in Header
**What goes wrong:** Page is created but nav link is missing — visitors can't reach the page.
**How to avoid:** Add the About nav link in the same PR/plan as the page creation. The link should go between Projects and (end of nav), using the same `class`, `href`, and styling as Home/Projects links in `Header.astro`.

### Pitfall 2: Wrong Route File Location
**What goes wrong:** File placed in wrong directory or wrong name.
**How to avoid:** `src/pages/about.astro` → `/about`. Do NOT create `src/pages/about/index.astro` unless the file is at a different nesting level. Either works, but `about.astro` is simpler and follows the project's pattern for single pages.

### Pitfall 3: Mismatched Theme Tokens
**What goes wrong:** Using hardcoded colors that differ from Phase 1's established palette.
**How to avoid:** Reuse exact color values from existing components: `bg-[#16213e]`, `text-[#e8e8e8]`, `text-[#a0a0b0]`, `text-[#4a90d9]`, `hover:text-[#6bb3f0]`, `border-[#2a2a3e]`.

### Pitfall 4: Container Width Mismatch
**What goes wrong:** About page uses different max-width than homepage.
**How to avoid:** Use the same container class as `Home.astro`: `mx-auto max-w-[720px] w-full px-4 py-8`.

## Code Examples

### Profile Photo Slot (Placeholder Pattern)
```astro
---
// User will provide the file path during planning
// import profilePhoto from "../assets/profile-photo.jpg";
---

<!-- Use a circular, bordered image -->
<!--
<img
  src={profilePhoto.src}
  width="120"
  height="120"
  alt="Gleb Golov"
  class="rounded-full border-2 border-[#2a2a3e] mb-6"
/>
-->

<!-- Fallback: placeholder circle -->
<div class="w-28 h-28 rounded-full bg-[#16213e] border-2 border-[#2a2a3e] mb-6 flex items-center justify-center">
  <span class="text-3xl font-serif text-[#a0a0b0]">GG</span>
</div>
```

### Languages as Compact Tags
```astro
<div class="flex flex-wrap gap-2 mb-6">
  <span class="text-sm text-[#a0a0b0]">English</span>
  <span class="text-[#2a2a3e]">·</span>
  <span class="text-sm text-[#a0a0b0]">Italian</span>
  <span class="text-[#2a2a3e]">·</span>
  <span class="text-sm text-[#a0a0b0]">Russian</span>
</div>
```

### Location After Bio (Text Format)
```astro
<p class="font-sans text-sm text-[#a0a0b0] mt-2">📍 Turin, Italy</p>
```

### Skills Section with Categories and Tags
```astro
<hr class="border-[#2a2a3e] mb-6" />

<section class="mb-6">
  <h2 class="font-serif font-bold text-2xl text-[#e8e8e8] mb-4">Skills</h2>

  <h3 class="font-serif font-semibold text-lg text-[#e8e8e8] mb-3">Frontend</h3>
  <div class="flex flex-wrap gap-2 mb-6">
    <span class="px-3 py-1 text-sm bg-[#16213e] text-[#a0a0b0] rounded-full">React</span>
    <span class="px-3 py-1 text-sm bg-[#16213e] text-[#a0a0b0] rounded-full">TypeScript</span>
    <span class="px-3 py-1 text-sm bg-[#16213e] text-[#a0a0b0] rounded-full">Vue.js</span>
    <span class="px-3 py-1 text-sm bg-[#16213e] text-[#a0a0b0] rounded-full">CSS / Tailwind</span>
  </div>

  <h3 class="font-serif font-semibold text-lg text-[#e8e8e8] mb-3">Backend</h3>
  <div class="flex flex-wrap gap-2 mb-6">
    <span class="px-3 py-1 text-sm bg-[#16213e] text-[#a0a0b0] rounded-full">Node.js</span>
    <span class="px-3 py-1 text-sm bg-[#16213e] text-[#a0a0b0] rounded-full">Python</span>
    <span class="px-3 py-1 text-sm bg-[#16213e] text-[#a0a0b0] rounded-full">PostgreSQL</span>
  </div>

  <h3 class="font-serif font-semibold text-lg text-[#e8e8e8] mb-3">Tools & DevOps</h3>
  <div class="flex flex-wrap gap-2">
    <span class="px-3 py-1 text-sm bg-[#16213e] text-[#a0a0b0] rounded-full">Git</span>
    <span class="px-3 py-1 text-sm bg-[#16213e] text-[#a0a0b0] rounded-full">Docker</span>
    <span class="px-3 py-1 text-sm bg-[#16213e] text-[#a0a0b0] rounded-full">GitHub Actions</span>
  </div>
</section>
```

### Education Section (Most Recent First)
```astro
<hr class="border-[#2a2a3e] mb-6" />

<section class="mb-6">
  <h2 class="font-serif font-bold text-2xl text-[#e8e8e8] mb-4">Education</h2>

  <div class="mb-4">
    <h3 class="font-serif font-semibold text-lg text-[#e8e8e8]">Master's Degree in Computer Science</h3>
    <p class="font-sans text-sm text-[#a0a0b0]">University Name</p>
    <p class="font-sans text-sm text-[#a0a0b0]">2018 — 2020</p>
  </div>

  <div>
    <h3 class="font-serif font-semibold text-lg text-[#e8e8e8]">Bachelor's Degree in Computer Science</h3>
    <p class="font-sans text-sm text-[#a0a0b0]">University Name</p>
    <p class="font-sans text-sm text-[#a0a0b0]">2014 — 2018</p>
  </div>
</section>
```

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| Two-column layout (pre-Phase 1) | Single-column centered layout (Phase 1) | Phase 1 | About page uses same single-column 720px pattern |
| Astro `<Image />` for SVGs | Astro SVG components (Astro 5.7+) | Astro 5.7 | SVGs can now be used as components, but existing code uses `<img>` import pattern — stay consistent |

**Deprecated/outdated:**
- None — the entire stack was just established in Phase 1 with current Astro 5.x and Tailwind v4.

## Assumptions Log

| # | Claim | Section | Risk if Wrong |
|---|-------|---------|---------------|
| A1 | User will provide profile photo path during planning — research recommends a placeholder fallback | Standard Stack | User might want no photo — easily removed if so |

**If this table is empty:** N/A — one assumption noted.

## Open Questions

1. **Profile photo file path and format**
   - What we know: User will provide during planning (D-14). The photo goes at the top of the page (D-02).
   - What's unclear: Exact filename, format (jpg/png), dimensions, and whether user wants circular crop.
   - Recommendation: Use a placeholder `<div>` with initials as fallback, gated behind a comment. The planner should add a `checkpoint:human-verify` task for the user to confirm/upload the photo, then a low-effort swap.

2. **Exact education entries**
   - What we know: Simple format (degree, institution, year), most recent first (D-12, D-13).
   - What's unclear: Actual degrees, institutions, and years.
   - Recommendation: Use plausible placeholder entries in About.astro (e.g., "Master's Degree in Computer Science", "University Name", "2018 — 2020"). The user can edit during or after planning.

3. **Exact skill technologies**
   - What we know: Categories are Frontend, Backend, Tools & DevOps (D-07). Standard web technologies (D-08). No proficiency levels (D-09).
   - What's unclear: The exact technologies per category.
   - Recommendation: Use common placeholder tech for each category (React, TypeScript, Vue.js, CSS/Tailwind for Frontend; Node.js, Python, PostgreSQL for Backend; Git, Docker, GitHub Actions for Tools & DevOps). User refines later.

## Environment Availability

> This phase has no external dependencies beyond what's already required by the project.

| Dependency | Required By | Available | Version | Fallback |
|------------|------------|-----------|---------|----------|
| Node.js | Astro build | ✓ | v24.11.1 | — |
| npm | Package management | ✓ | 11.14.1 | — |
| Astro | Static site generation | ✓ (in node_modules) | ^5.7.5 | — |

**Missing dependencies with no fallback:** None
**Missing dependencies with fallback:** None

## Validation Architecture

> Skipped — `workflow.nyquist_validation` is explicitly `false` in `.planning/config.json`.

## Security Domain

> This phase adds only static content and a navigation link. No user input, no data processing, no authentication, no external APIs.
> All ASVS categories: not applicable.
> Security review: not required — pure markup change.

## Recommended Plan Split

The phase requirements map naturally into 3 execution plans:

### Plan A: Create About Page Route + Component (ABOUT-01, ABOUT-03)
**Scope:** `src/pages/about.astro` + `src/components/About.astro`
- Create `src/pages/about.astro` following the `projects/index.astro` pattern (Layout → Header → About → Footer)
- Create `src/components/About.astro` with:
  - Profile photo placeholder slot (circular initials fallback)
  - Bio section: 2-3 professional paragraphs (placeholder CV summary)
  - Location: "Turin, Italy" after the bio
  - Languages: compact dot-separated list (English, Italian, Russian)
  - Section dividers (`<hr class="border-[#2a2a3e]">`) between sections
  - Single-column centered layout: `mx-auto max-w-[720px] w-full px-4 py-8`
- Education section: 2 entries (most recent first), simple degree/institution/year format
- All theme tokens matching Phase 1 (colors, typography, spacing)

### Plan B: Add Skills Section (ABOUT-02)
**Scope:** Add skills to `About.astro` (between Bio and Education)
- Skills section with 3 category subheadings: Frontend, Backend, Tools & DevOps
- Tag-style items using `bg-[#16213e]` rounded `px-3 py-1 text-sm` spans
- Placeholder technologies per category (user refines later)
- No proficiency levels or visual indicators

### Plan C: Wire Navigation Link (ABOUT-04)
**Scope:** `src/components/Header.astro`
- Add About `<a>` tag to the `<nav>` in `Header.astro`
- Use same class pattern as Home and Projects links
- Place after Projects link, before `</ul>`
- Confirm the aria-current attribute works when on the About page

### Execution Order
Wave 1: Plan A + Plan C (page + nav link — must ship together to avoid 404 link)
Wave 2: Plan B (skills section — can be added after page exists)

Total: 3 plans, 2 waves.

## Sources

### Primary (HIGH confidence)
- [CITED: docs.astro.build/en/core-concepts/routing/] — Astro file-based routing: `src/pages/about.astro` → `/about`
- [CITED: docs.astro.build/en/basics/layouts/] — Layout component pattern with `<slot />`
- [CITED: docs.astro.build/en/guides/images/] — Image import patterns: `import img from "path"` → `{img.src}`
- [VERIFIED: codebase inspection] — All existing component patterns, theme tokens, and composition patterns confirmed by reading `Home.astro`, `Header.astro`, `Footer.astro`, `Layout.astro`, `projects/index.astro`

### Secondary (MEDIUM confidence)
- [CITED: docs.astro.build/en/guides/images/#svg-components] — Astro 5.7+ SVG component support (not needed for this phase)

### Tertiary (LOW confidence)
- None — all findings verified against official docs or existing codebase.

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — no new packages needed, existing stack verified via `package.json` and codebase inspection
- Architecture: HIGH — patterns verified against 5 existing Astro components
- Pitfalls: HIGH — all sourced from observed bugs in existing codebase (CONVENTIONS.md)
- Recommended plan split: MEDIUM — organization decision, can be adjusted

**Research date:** 2026-05-19
**Valid until:** 2026-06-19 (30 days — stable stack, no fast-moving dependencies)
