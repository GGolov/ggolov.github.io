# Gleb Golov — Personal CV Site

## What This Is

A personal curriculum vitae website for Gleb Golov, a Web Developer. The site presents a professional resume-style landing page with work experience, skills, education, and bio sections — styled with a classic, dark-neutral aesthetic to make a strong impression on potential employers and recruiters. Built as a static Astro site hosted on GitHub Pages.

## Core Value

Visitors can quickly understand Gleb's professional background, skills, and experience in a clean, readable format that feels like a well-designed resume.

## Requirements

### Validated

- ✓ Homepage with name, title, and experience section — existing
- ✓ Projects page with basic route — existing
- ✓ Navigation between Home and Projects pages — existing
- ✓ Footer with hosting credits — existing
- ✓ Social links to LinkedIn and GitHub — existing
- ✓ Responsive HTML shell with Tailwind CSS — existing
- ✓ Deployed to GitHub Pages via GitHub Actions — existing

### Active

- [ ] **Redesign homepage** with a classic resume single-column layout and dark/neutral color scheme
- [ ] **Create About page** with bio, skills/tech stack, and education sections
- [ ] **Fix invalid HTML** in Home.astro (nested `<li>` inside `<li>`)
- [ ] **Remove Supabase mention** from footer (not actually used)
- [ ] **Fix the About nav link** so it points to the new About page (currently 404)
- [ ] **Update global styling** — professional dark/neutral theme, serif or clean typography, paper-like feel appropriate for a CV
- [ ] **Add skills section** to homepage or About page with technology categories
- [ ] **Add education section** to the About page

### Out of Scope

- Projects page redesign — deferred; keep existing placeholder for now
- Contact form — not needed for v1; email/social links sufficient
- Client-side interactivity (JS frameworks, animations) — keep static
- Backend APIs or server routes — site remains fully static
- Blog or articles section — not part of CV scope
- Testimonials / recommendations — no content available yet

## Context

- Built with Astro 5.x and Tailwind CSS v4
- Currently has an amber-toned color scheme (amber-100/400) that needs replacement
- Homepage uses a two-column layout (sidebar + experience); redesigning to single-column
- The existing `src/components/Home.astro` has an HTML nesting bug (`<li>` inside `<li>`)
- The footer credits Supabase which is not a dependency — needs cleaning up
- Navigation has an About link that leads to a 404 page
- The Projects page is a placeholder with just the text "Projects"
- No tests, no linting, no type-checking CI step — these could be addressed but aren't blocking
- Site URL: https://ggolov.github.io
- Repository: https://github.com/GGolov/ggolov.github.io

## Constraints

- **Static only**: No backend, no server-side rendering beyond Astro SSG — must be deployable to GitHub Pages
- **Framework**: Must stay within Astro + Tailwind CSS — no new build tools unless unavoidable
- **Maintainability**: Should be easy for AI agents to continue editing — keep component structure simple

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Classic resume style | Professional CV impression, timeless layout | — Pending |
| Single column layout | Clean top-to-bottom reading flow for recruiters | — Pending |
| Dark/neutral color scheme | Modern professional look, high readability | — Pending |
| Create About page (not remove link) | Natural home for bio/skills/education content | — Pending |
| Remove Supabase from footer | Not used, misleading credit | — Pending |

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
*Last updated: 2026-05-19 after initialization*
