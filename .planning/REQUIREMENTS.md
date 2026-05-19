# Requirements: Gleb Golov — Personal CV Site

**Defined:** 2026-05-19
**Core Value:** Visitors can quickly understand Gleb's professional background, skills, and experience in a clean, readable format that feels like a well-designed resume.

## v1 Requirements

Requirements for initial redesign. Each maps to roadmap phases.

### Visual Redesign

- [x] **VISUAL-01**: Apply a dark/neutral color scheme across the entire site — dark background, light text, minimal accent colors
- [x] **VISUAL-02**: Use classic resume typography — serif or clean professional fonts appropriate for a CV
- [x] **VISUAL-03**: Redesign homepage to single-column layout with professional resume-style spacing
- [x] **VISUAL-04**: Ensure high readability and accessibility with proper contrast ratios

### Homepage

- [x] **HOME-01**: Replace current two-column layout with single-column classic resume flow
- [x] **HOME-02**: Display name, title, and professional summary/bio at the top
- [x] **HOME-03**: Show work experience in a clear chronological resume format
- [x] **HOME-04**: Fix invalid HTML nesting (current `<li>` inside `<li>` issue in `Home.astro`)
- [x] **HOME-05**: Integrate social links (LinkedIn, GitHub) into the header or a dedicated contact bar

### About Page

- [ ] **ABOUT-01**: Create `/about` page with professional bio section
- [ ] **ABOUT-02**: Add skills/tech stack section with categorized technologies
- [ ] **ABOUT-03**: Add education section with academic background
- [ ] **ABOUT-04**: Fix the About link in the header navigation (currently leads to 404)

### Header & Footer

- [x] **HEADER-01**: Redesign header to match dark/neutral theme, include nav and contact links
- [x] **HEADER-02**: Ensure nav links are correct (Home, Projects, About all resolve)
- [x] **FOOTER-01**: Remove Supabase credit from footer (not actually used)
- [x] **FOOTER-02**: Redesign footer to match dark/neutral theme

## v2 Requirements

Deferred to future iteration. Tracked but not in current roadmap.

### Projects

- **PROJ-01**: Redesign Projects page with real project showcases
- **PROJ-02**: Add project detail view or expandable project cards

### Additional

- **CONTACT-01**: Add a contact form or contact section
- **CONTACT-02**: Add downloadable resume PDF link
- **SKILL-01**: Add visual skill indicators (bars, tags, or icons)
- **TEST-01**: Add basic component rendering tests
- **CI-01**: Add type-checking step to GitHub Actions workflow

## Out of Scope

| Feature | Reason |
|---------|--------|
| Contact form (v1) | Social links sufficient for initial redesign |
| Blog/articles | Not part of CV scope |
| Client-side JS frameworks | Site remains fully static |
| Backend APIs or server routes | Static site only, GitHub Pages compatible |
| Testimonials/references | No content available yet |
| Downloadable resume PDF | Deferred to v2 |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| VISUAL-01 | Phase 1 | Complete |
| VISUAL-02 | Phase 1 | Complete |
| VISUAL-03 | Phase 1 | Complete |
| VISUAL-04 | Phase 1 / Phase 3 | Complete |
| HOME-01 | Phase 1 | Complete |
| HOME-02 | Phase 1 | Complete |
| HOME-03 | Phase 1 | Complete |
| HOME-04 | Phase 1 | Complete |
| HOME-05 | Phase 1 | Complete |
| ABOUT-01 | Phase 2 | Pending |
| ABOUT-02 | Phase 2 | Pending |
| ABOUT-03 | Phase 2 | Pending |
| ABOUT-04 | Phase 2 | Pending |
| HEADER-01 | Phase 1 | Complete |
| HEADER-02 | Phase 1 | Complete |
| FOOTER-01 | Phase 1 | Complete |
| FOOTER-02 | Phase 1 | Complete |

**Coverage:**
- v1 requirements: 17 total
- Mapped to phases: 17
- Unmapped: 0 ✓

---
*Requirements defined: 2026-05-19*
*Last updated: 2026-05-19 after initialization*
