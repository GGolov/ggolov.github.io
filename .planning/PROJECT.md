# Gleb Golov — Personal CV Site

## What This Is

A personal curriculum vitae website for Gleb Golov, a Web Developer. The site presents a professional resume-style landing page with work experience, skills, education, and bio sections — styled with a classic, dark-neutral aesthetic to make a strong impression on potential employers and recruiters. Built as a static Astro site hosted on GitHub Pages. v1.0 shipped May 2026.

## Core Value

Visitors can quickly understand Gleb's professional background, skills, and experience in a clean, readable format that feels like a well-designed resume.

## Current State (v2.0 — in progress)

The site has been consolidated to a single-page CV experience. The About page was merged into Home and removed. The site now has:

- **2 pages**: Home, Projects — all with consistent dark/neutral theme
- **Dark theme**: `#0f0f0f` background, `#e8e8e8` text, `#4a90d9` accent
- **Single-column layout**: `max-w-[720px]` centered container on all content pages
- **Header**: Serif "GG" logo, nav links (Home, Projects), social icons (LinkedIn, GitHub)
- **Footer**: Dark surface, muted text, hosting credits (Astro + GitHub Pages)
- **Homepage**: Name, title, photo placeholder, bio (3 paragraphs), languages, skills (3 categories), experience (2 entries), education (2 entries), social links — all CV info in one page
- **Projects page**: Styled placeholder with "Coming soon" message
- **Accessibility**: Skip-to-content link, semantic HTML landmarks, `rel="noopener noreferrer"` on external links, WCAG AA contrast
- **Tech**: Astro 5.x, Tailwind CSS v4, static output, ~500 LOC across source files

## Requirements

### Validated (v1.0)

- ✓ **VISUAL-01** — Dark/neutral color scheme — v1.0
- ✓ **VISUAL-02** — Classic resume typography (system serif/sans stack) — v1.0
- ✓ **VISUAL-03** — Single-column layout — v1.0
- ✓ **VISUAL-04** — High readability and accessibility (WCAG AA) — v1.0
- ✓ **HOME-01** — Single-column resume flow — v1.0
- ✓ **HOME-02** — Name, title, bio at top — v1.0
- ✓ **HOME-03** — Chronological work experience — v1.0
- ✓ **HOME-04** — Fix HTML nesting bug — v1.0
- ✓ **HOME-05** — Social links in header — v1.0
- ✓ **ABOUT-01** — Create `/about` with bio — v1.0
- ✓ **ABOUT-02** — Skills/tech stack section — v1.0
- ✓ **ABOUT-03** — Education section — v1.0
- ✓ **ABOUT-04** — Fix About nav link — v1.0
- ✓ **ABOUT-05** — Merge About CV info into Home page, delete About page — v2.0
- ✓ **HEADER-01** — Redesign header — v1.0
- ✓ **HEADER-02** — Fix all nav links — v1.0
- ✓ **FOOTER-01** — Remove Supabase credit — v1.0
- ✓ **FOOTER-02** — Redesign footer — v1.0

### Active

- [ ] Real project content for Projects page
- [ ] Downloadable resume PDF
- [ ] Contact section or contact form

### Out of Scope

- Projects page redesign — deferred to v2; placeholder sufficient
- Contact form — not needed for v1; email/social links sufficient (revisit for v2)
- Client-side interactivity (JS frameworks, animations) — keep static
- Backend APIs or server routes — site remains fully static
- Blog or articles section — not part of CV scope
- Testimonials / recommendations — no content available yet
- Automated CI tests — not blocking for initial release
- Dedicated About page — merged into Home as single-page CV (v2.0)

## Context

- Built with Astro 5.x and Tailwind CSS v4
- Deployed to GitHub Pages via GitHub Actions
- ~400 LOC across src/ (6 Astro components, 1 layout, 1 CSS file, 3 pages)
- Site URL: https://ggolov.github.io
- Repository: https://github.com/GGolov/ggolov.github.io

## Constraints

- **Static only**: No backend, no server-side rendering beyond Astro SSG — must be deployable to GitHub Pages
- **Framework**: Must stay within Astro + Tailwind CSS — no new build tools unless unavoidable
- **Maintainability**: Should be easy for AI agents to continue editing — keep component structure simple

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Classic resume style | Professional CV impression, timeless layout | ✓ Implemented v1.0 |
| Single column layout | Clean top-to-bottom reading flow for recruiters | ✓ Implemented v1.0 |
| Dark/neutral color scheme | Modern professional look, high readability | ✓ Implemented v1.0 |
| Create About page (not remove link) | Natural home for bio/skills/education content | ✓ Implemented v1.0 |
| Remove Supabase from footer | Not used, misleading credit | ✓ Implemented v1.0 |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition:**
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone:**
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-05-19 after merging About into Home (v2.0)*
