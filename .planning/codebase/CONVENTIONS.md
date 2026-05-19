# Coding Conventions

**Analysis Date:** 2026-05-19

## Naming Patterns

**Files:**
- **Components** — PascalCase: `Home.astro`, `Header.astro`, `Footer.astro`, `Projects.astro`
- **Layouts** — PascalCase: `Layout.astro`
- **Pages** — `index.astro` (lowercase, uses file-based routing)
- **Static assets** — kebab-case: `linkedin-icon.svg`, `github-icon.svg`, `background.svg`, `favicon.svg`
- **Styles** — kebab-case: `global.css`
- **Config files** — kebab-case: `astro.config.mjs`, `.github/copilot-instructions.md`
- **Images from `src/assets/`** imported as Astro asset references, then accessed via `.src` property

**Components:**
- Functional, stateless components — no client-side props, no scripts, no interactivity
- No TypeScript types defined in components (all are plain Astro frontmatter with no explicit interface)

**CSS Classes:**
- Tailwind utility classes only (no custom CSS classes defined)
- Classes use Tailwind syntax: `flex`, `flex-row`, `p-3`, `font-bold`, `bg-amber-400`, etc.
- Custom CSS selectors: only the universal `* { transition-duration: 200ms; }` in `global.css`

## Component Structure

**Standard pattern** (e.g., `Header.astro`, `Footer.astro`):
```astro
---
// Optional frontmatter (empty for Header, Footer; imports for Home)
---
<!-- Template (HTML + Tailwind classes) -->

<style></style>  <!-- Present but empty in all components -->
```

**Detailed pattern** (e.g., `Home.astro`):
```astro
---
import linkedinIcon from "../assets/linkedin-icon.svg";
import gitHubIcon from "../assets/github-icon.svg";
---
<!-- HTML template with Tailwind classes -->
```

**Page pattern** (e.g., `index.astro`, `projects/index.astro`):
```astro
---
import Layout from "../layouts/Layout.astro";
import Header from "../components/Header.astro";
import Footer from "../components/Footer.astro";
// ... page-specific component imports
---

<Layout>
  <Header />
  <!-- Page content -->
  <Footer />
</Layout>
```

## Import Organization

**Import order (consistent across all files):**
1. Styles (`import "../styles/global.css"`)
2. Components/Layouts (relative `../components/` and `../layouts/`)
3. Assets (relative `../assets/`)

**All imports use relative paths** (no path aliases configured in `tsconfig.json` beyond Astro defaults).

## Asset Conventions

- SVG icons imported from `src/assets/` using standard Astro image import (`import icon from "../assets/icon.svg"`)
- Accessed via `.src` property: `{icon.src}`
- Favicon linked from `public/favicon.svg` in `Layout.astro`
- Inline attributes used: `width`, `height` on `<img>` tags
- No `alt` text on social icon images in `Home.astro` (`<img src={linkedinIcon.src} width="30" height="30" />`) — accessibility gap

## CSS Conventions

**Global styles** (`src/styles/global.css`):
```css
@import "tailwindcss";

* {
  transition-duration: 200ms;
}
```

- Tailwind CSS v4 via `@import "tailwindcss"` directive
- Single global CSS file imported in `Layout.astro`
- No component-scoped styles (empty `<style></style>` tags in all `.astro` components)
- No CSS variables, no custom properties defined
- All styling done inline via Tailwind utility classes
- Color palette: uses Tailwind's `amber` family (`amber-100`, `amber-400`, `amber-500`, `amber-600`)

## HTML Conventions

**Semantic elements used:**
- `<header>`, `<footer>`, `<nav>`, `<main>`, `<aside>`, `<section>` — all used appropriately
- `<ol>` / `<li>` for ordered experience list
- `<ul>` / `<li>` for navigation and social links
- `<h1>` through `<h3>` for headings
- `<p>` for paragraphs
- `<span>` for inline text

**Doctype and structure:**
- Standard `<!doctype html>` with `<html lang="en">`
- Viewport meta tag: `<meta name="viewport" content="width=device-width" />`

## Link Conventions

**External links** — Should follow pattern from `AGENTS.md`:
```astro
<a href="..." target="_blank" rel="noopener noreferrer">...</a>
```

**Convention violations found:**
- `Footer.astro`: The "Astro" link has `target="_blank"` but **missing** `rel="noopener noreferrer"`
- `Footer.astro`: The "GitHub Pages" link has `target="_blank"` but **missing** `rel="noopener noreferrer"`

**Internal links** — No `target="_blank"` (correct):
- `<a href="/">Home</a>`, `<a href="/projects">Projects</a>`, `<a href="/about">About</a>` in `Header.astro`

## Potential Issues / Convention Violations

### HTML Nesting Violation
`src/components/Home.astro` (lines ~62-77): A `<li>` is nested inside another `<li>`, which is invalid HTML:
```html
<li class="flex flex-row gap-2">
  <section>...</section>
  <li class="flex flex-row gap-2">  <!-- INVALID: nested li -->
    ...
  </li>
</li>
```
The fourth placeholder `<li>` should be a sibling, not a child of the third `<li>`.

### Placeholder Content
- `Home.astro`: Contains "Placeholder Role" and "Fourth Placeholder Role" entries with fake company names and dates
- `Projects.astro`: Entire component body is just the text "Projects"

### Empty `<style>` Tags
All four components (`Home.astro`, `Header.astro`, `Footer.astro`, `Projects.astro`) contain `<style></style>` tags with no content. These should be removed to reduce noise.

### Entity Encoding Issue
`Header.astro` line 7 contains `&#38;nbsp;` which renders as literal `&nbsp;` text instead of a non-breaking space. Should be `&nbsp;` (or the element removed entirely — it appears to be a visual spacer).

### Broken Link
`Header.astro` links to `/about` which has no corresponding page route (`src/pages/about/` doesn't exist).

### Missing Alt Text on Images
`Home.astro` social icon `<img>` tags have no `alt` attribute, which is an accessibility concern.

### Supabase Mention
`Footer.astro` references Supabase ("Powered by Astro and Supabase") but Supabase is not used anywhere in the project. This appears to be a copy-paste error.

### No TypeScript Interfaces
No TypeScript types or interfaces are defined despite `tsconfig.json` extending `astro/tsconfigs/strict`. Components have no props interfaces, even where data structures like experience items exist.

---

*Convention analysis: 2026-05-19*
