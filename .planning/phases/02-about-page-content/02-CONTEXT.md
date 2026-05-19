# Phase 2: About Page & Content — Context

**Gathered:** 2026-05-19
**Status:** Ready for planning

<domain>
## Phase Boundary

Create a new `/about` route with professional bio, categorized skills/tech stack, and education sections. Wire it into the header navigation. The page must match the dark/neutral theme and single-column centered layout established in Phase 1.

**Requirements (from REQUIREMENTS.md):**
- ABOUT-01: Create `/about` page with professional bio section
- ABOUT-02: Add skills/tech stack section with categorized technologies
- ABOUT-03: Add education section with academic background
- ABOUT-04: Fix the About link in the header navigation (currently leads to 404)

</domain>

<decisions>
## Implementation Decisions

### Section Order & Page Structure
- **D-01:** Sections appear top-to-bottom as: Bio → Skills → Education
- **D-02:** Include a profile photo/avatar at the top of the page (user will provide the file path during planning)
- **D-03:** Include location (Turin, Italy) after the bio — shown in text form, not as a separate section
- **D-04:** Include a languages section — list of languages spoken (e.g., English, Italian, Russian) as a compact list or tags
- **D-05:** Single-column centered layout (~720px max-width), matching the homepage design from Phase 1
- **D-06:** No contact section on the About page — the Contact section on the homepage is sufficient

### Skills Section Format
- **D-07:** Skills organized by category (Frontend, Backend, Tools & DevOps) — each category as a subheading with tag-style items underneath
- **D-08:** Use standard web development categories and technologies as placeholder content for now — user can refine the exact list later
- **D-09:** No proficiency levels or visual indicators — simple tag-style listing

### Bio Depth & Tone
- **D-10:** Professional CV summary — concise, formal, 2-3 paragraphs
- **D-11:** Similar tone to LinkedIn summary — covers role, expertise, and professional approach

### Education Section Format (the agent's discretion)
- **D-12:** Simple format — degree, institution, year per entry. Clean and scannable, no descriptions or achievements.
- **D-13:** Chronological order (most recent first)

### Profile Photo (pending)
- **D-14:** User will provide the photo file path during planning — add a placeholder or configurable image slot

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Requirements & Roadmap
- `.planning/REQUIREMENTS.md` — Requirements ABOUT-01 through ABOUT-04 with traceability
- `.planning/ROADMAP.md` — Phase 2 goal and success criteria

### Codebase Conventions & Architecture
- `.planning/codebase/CONVENTIONS.md` — Coding conventions (naming, component structure, import patterns)
- `.planning/codebase/STRUCTURE.md` — Where to add new page/component (see "Where to Add New Code" section)
- `.planning/codebase/STACK.md` — Technology stack (Astro, Tailwind CSS v4)

### Source Files (read during planning)
- `src/styles/global.css` — CSS custom properties and theme tokens
- `src/components/Home.astro` — Reference for component patterns, layout, and styling conventions
- `src/components/Header.astro` — Needs About link added to navigation
- `src/pages/index.astro` — Reference for page composition pattern (Layout + Header + content + Footer)
- `src/layouts/Layout.astro` — Page shell

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- **Page composition pattern** (`src/pages/index.astro`): Layout wraps Header + content + Footer — same pattern for the new About page
- **Styling tokens** (`src/styles/global.css`): CSS custom properties (`--color-bg`, `--color-text`, `--color-accent`, etc.) are reusable directly
- **Single-column layout** (`src/components/Home.astro`): `flex-auto mx-auto max-w-[720px] w-full px-4 py-8` — reusable container pattern

### Established Patterns
- **Serif headings, sans-serif body** — use `font-serif` for section titles, `font-sans` for body text
- **Section dividers** — `<hr class="border-[#2a2a3e]">` between major sections
- **SVG asset imports** — import from `src/assets/` and use `.src` property
- **Component-per-section** pattern — can create `About.astro` component analogous to `Home.astro`

### Integration Points
- **Header navigation** — add `/about` link to the `<nav>` in `Header.astro`
- **New route** — create `src/pages/about.astro` (or `src/pages/about/index.astro`)
- **New component** — create `src/components/About.astro` for the page content

</code_context>

<specifics>
## Specific Ideas

No specific references were mentioned during discussion — standard CV/about page approach is expected.

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 2-About-Page-Content*
*Context gathered: 2026-05-19*
