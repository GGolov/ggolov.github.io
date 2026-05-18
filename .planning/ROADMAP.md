# Roadmap: Gleb Golov — Personal CV Site

**3 phases** | **17 requirements mapped** | All v1 requirements covered ✓

---

## Phase 1: Visual Foundation & Homepage Redesign

**Goal:** Apply the dark/neutral professional theme across the site and redesign the homepage as a single-column classic resume layout.

**Mode:** mvp

**Requirements:**
- VISUAL-01: Dark/neutral color scheme
- VISUAL-02: Classic resume typography
- VISUAL-03: Single-column layout
- VISUAL-04: High readability and accessibility
- HOME-01: Single-column resume flow
- HOME-02: Name, title, bio at top
- HOME-03: Chronological work experience
- HOME-04: Fix HTML nesting bug
- HOME-05: Social links in header/contact bar
- HEADER-01: Redesign header
- HEADER-02: Fix all nav links
- FOOTER-01: Remove Supabase credit
- FOOTER-02: Redesign footer

**Success Criteria:**
1. Homepage renders in single-column classic resume layout with dark background and light text
2. Header and footer use the new dark/neutral theme with correct navigation
3. All nav links resolve (Home works, Projects exists, About doesn't need to exist yet)
4. No HTML validation errors in `Home.astro`
5. Supabase removed from footer
6. Social links (LinkedIn, GitHub) are visible and functional

---

## Phase 2: About Page & Content

**Goal:** Create the About page with bio, skills/tech stack, and education sections. Wire it into the navigation.

**Mode:** mvp

**Requirements:**
- ABOUT-01: Create `/about` with bio
- ABOUT-02: Skills/tech stack section
- ABOUT-03: Education section
- ABOUT-04: Fix About nav link

**Success Criteria:**
1. `/about` page exists and loads without 404
2. About page includes bio paragraph, skills categories with technologies, and education history
3. Header About link navigates to the new page
4. About page matches the dark/neutral theme from Phase 1

---

## Phase 3: Polish & Quality

**Goal:** Final refinement — accessibility audit, visual consistency check, and cleanup of any remaining rough edges.

**Mode:** mvp

**Requirements:**
- VISUAL-04: Accessibility and contrast verification

**Success Criteria:**
1. Color contrast ratios meet WCAG AA standards
2. All pages have consistent spacing, typography, and styling
3. Site builds without errors (`npm run build` succeeds)
4. Responsive layout works on mobile and desktop viewports

---

## Phase Mapping

```
Phase 1 ──┬── VISUAL-01, VISUAL-02, VISUAL-03, VISUAL-04
           ├── HOME-01, HOME-02, HOME-03, HOME-04, HOME-05
           ├── HEADER-01, HEADER-02
           └── FOOTER-01, FOOTER-02

Phase 2 ──┬── ABOUT-01, ABOUT-02, ABOUT-03, ABOUT-04

Phase 3 ──└── VISUAL-04 (verification)
```

---

## Coverage

| Category | Total | Mapped | Unmapped |
|----------|-------|--------|----------|
| Visual Redesign | 4 | 4 | 0 ✓ |
| Homepage | 5 | 5 | 0 ✓ |
| About Page | 4 | 4 | 0 ✓ |
| Header & Footer | 4 | 4 | 0 ✓ |
| **Total** | **17** | **17** | **0 ✓** |
