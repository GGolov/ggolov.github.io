# Codebase Concerns

**Analysis Date:** 2026-05-19

## Summary

This is a small personal static site built with Astro + Tailwind CSS. While functional, it has several quality-of-life issues, placeholder content, and missing features that should be prioritized. The top 3 items to fix are:

1. **About page 404** — Header links to `/about` but no page exists
2. **Invalid HTML in Home.astro** — Nested `<li>` elements produce invalid DOM
3. **Placeholder content** — Two experience entries contain fake placeholder data

---

## HIGH Severity

| # | Issue | Category | Location | Description | Recommended Action |
|---|-------|----------|----------|-------------|-------------------|
| 1 | **About page missing → 404** | Bug | `src/components/Header.astro:13` | Header nav links to `/about` but no `src/pages/about.astro` exists. Any visitor clicking "About" gets a 404. | Create `src/pages/about.astro` with real content, or remove the nav link. |
| 2 | **Invalid HTML nesting in experience list** | Bug | `src/components/Home.astro:56-73` | A `<li>` (line 56) contains another `<li>` (line 67) as a direct child, which is invalid HTML. Browsers may auto-close the first `<li>` early, breaking layout semantics. | Restructure so all `<li>` elements are siblings within the `<ol>`. The nested `<li>` block should be moved outside its parent `<li>`. |

## MEDIUM Severity

| # | Issue | Category | Location | Description | Recommended Action |
|---|-------|----------|----------|-------------|-------------------|
| 3 | **Placeholder content visible on homepage** | Tech-debt | `src/components/Home.astro:45-73` | Two experience entries use "Placeholder Role", "Placeholder Company", "Placeholder Date", and "Fourth Placeholder Role". These appear on the live site. | Replace with real experience entries or remove the placeholder items entirely. |
| 4 | **Supabase referenced but not used** | Bug | `src/components/Footer.astro:5-10` | Footer states "Powered by Astro and Supabase and hosted on GitHub Pages". Supabase is not in `package.json` dependencies, not configured anywhere in the project, and has no backend/API to power. It is misleading. | Remove "Supabase" from the footer text, or add actual Supabase integration if intended. |
| 5 | **Projects page is empty** | Missing-feature | `src/components/Projects.astro:1-2` | The `/projects` page renders only the text "Projects" inside a `<main>` element. No project cards, list, or any content. | Populate with real project entries, or redirect to homepage. |
| 6 | **No type-checking in CI pipeline** | Tech-debt | `.github/workflows/deploy.yml` | The deploy workflow uses `withastro/action@v3` to build but does not run `astro check` or any TypeScript type validation before deployment. TypeScript `strict` mode is enabled in `tsconfig.json` but never verified. | Add a step before build that runs `npx astro check` or `tsc --noEmit`. Alternatively add a separate lint/type-check job. |
| 7 | **No linting or formatting configuration** | Maintenance | (root) | No ESLint, Prettier, or Biome configuration files exist anywhere in the project. Code style is inconsistent by default. | Add a linting/formatting tool (Prettier is simplest for Astro). Run `astro add` for official linter integration. |
| 8 | **No meta description for SEO** | Missing-feature | `src/layouts/Layout.astro:5-10` | The `<head>` only has charset, viewport, favicon, and generator meta tags. No `<meta name="description">`, Open Graph tags, or Twitter Card tags. | Add a meta description and Open Graph tags to `Layout.astro`. Use Astro's `define:vars` or props to make them page-specific. |

## LOW Severity

| # | Issue | Category | Location | Description | Recommended Action |
|---|-------|----------|----------|-------------|-------------------|
| 9 | **README is default Astro starter** | Maintenance | `README.md` | The README contains the generic Astro "Basics" starter template with instructions to "delete this file". It does not describe the actual project. | Replace with a project-specific README describing the personal site, how to run it, and deployment info. |
| 10 | **No 404 error page** | Missing-feature | (root) | No `src/pages/404.astro` exists. Astro will use its built-in 404 page when a route is not found, but a custom 404 with navigation links would improve UX. | Create `src/pages/404.astro` with branding and links back to working pages. |
| 11 | **No RSS feed** | Missing-feature | (root) | Astro supports RSS feed generation out of the box, but no RSS feed is configured. For a personal site this is optional but expected by some visitors. | Consider adding `@astrojs/rss` if any content is added that warrants syndication. |
| 12 | **No sitemap** | Missing-feature | (root) | No sitemap.xml is generated. This is minor for a 2-page site but best practice for any public website. | Add `@astrojs/sitemap` integration. |
| 13 | **Default Astro favicon** | Maintenance | `public/favicon.svg` | The favicon is the standard Astro rocket logo, not personalized to the site owner. | Replace with a custom favicon (e.g., initials "GG" or a personal logo). |
| 14 | **Empty OpenSpec directories** | Maintenance | `openspec/specs/`, `openspec/changes/` | The `openspec/` directory has an empty `specs/` folder and empty `changes/` (with empty `archive/`). These appear to be scaffolding that was never populated. | Either populate with actual specs/changes or remove the unused directories. |
| 15 | **No tests or testing dependencies** | Maintenance | `package.json` | No test runner, no test files, no testing configuration. While acceptable for a simple static site, any future interactive features would lack coverage. | Not urgent, but plan to add Vitest + `@astrojs/check` when adding interactive features. |
| 16 | **Unused background.svg asset** | Maintenance | `src/assets/background.svg` | An SVG file exists in assets but is not imported or referenced by any component. | Remove the unused asset, or use it as a background image on the homepage. |
| 17 | **Global transition on all elements** | Tech-debt | `src/styles/global.css:4` | The `* { transition-duration: 200ms; }` applies a transition to every element on the page. This can cause unexpected visual effects on page load or with dynamically added elements. | Scope transitions to specific interactive elements (e.g., `a, button, input`) to avoid side effects. |
| 18 | **Social icon size mismatch** | Tech-debt | `src/components/Home.astro:24,32` | LinkedIn icon SVG has native `width="24"` and `height="24"` but is rendered at `width="30"` `height="30"`. GitHub icon SVG has native `width="32"` `height="32"` and is also rendered at 30×30. Minor scaling inconsistency. | Either use consistent native dimensions or remove the hardcoded SVG sizes and rely on the rendering width/height. |
| 19 | **No responsive design patterns** | Missing-feature | `src/components/Home.astro`, `src/components/Header.astro` | The layout uses `flex-row` without responsive breakpoints. On mobile the sidebar and experience section will appear side-by-side rather than stacked, likely causing layout issues. | Add responsive breakpoints (e.g., `flex-col md:flex-row`) and test on mobile viewports. |

## Security Considerations

| # | Concern | Location | Details | Recommendation |
|---|---------|----------|---------|----------------|
| S1 | **External link safety — one link missing `rel="noopener noreferrer"`** | `src/components/Footer.astro:4` | The Astro link (`astro.build`) is missing `target="_blank"` and `rel="noopener noreferrer"`. This is low risk (reputable site) but inconsistent with the other external links. | Add `target="_blank" rel="noopener noreferrer"` to the Astro link for consistency. |
| S2 | **No dependency audit in CI** | `.github/workflows/deploy.yml` | No `npm audit` or Dependabot configuration. Supply-chain vulnerabilities could go unnoticed. | Add `npm audit` step to CI or enable Dependabot in repo settings. |
| S3 | **No CSP headers** | `astro.config.mjs` | No Content Security Policy is configured. For a static personal site this is low-risk, but still a best practice. | Add CSP via Astro's `response.headers` or a hosting platform config. |

---

*Concerns audit: 2026-05-19*
