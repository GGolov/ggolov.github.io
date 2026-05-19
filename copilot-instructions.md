<!-- GSD:project-start source:PROJECT.md -->
## Project

**Gleb Golov — Personal CV Site**

A personal curriculum vitae website for Gleb Golov, a Web Developer. The site presents a professional resume-style landing page with work experience, skills, education, and bio sections — styled with a classic, dark-neutral aesthetic to make a strong impression on potential employers and recruiters. Built as a static Astro site hosted on GitHub Pages.

**Core Value:** Visitors can quickly understand Gleb's professional background, skills, and experience in a clean, readable format that feels like a well-designed resume.

### Constraints

- **Static only**: No backend, no server-side rendering beyond Astro SSG — must be deployable to GitHub Pages
- **Framework**: Must stay within Astro + Tailwind CSS — no new build tools unless unavoidable
- **Maintainability**: Should be easy for AI agents to continue editing — keep component structure simple
<!-- GSD:project-end -->

<!-- GSD:stack-start source:codebase/STACK.md -->
## Technology Stack

## Languages
- **TypeScript** — Strict mode, used throughout components and config. Config via `astro/tsconfigs/strict` in `tsconfig.json`.
- **Astro (`.astro` files)** — Component islands combining HTML, frontmatter TypeScript, and scoped CSS in a single file. Compiled at build time to static HTML.
- **CSS** — Tailwind utility classes in templates, plus one global CSS rule in `src/styles/global.css`.
- **SVG** — Icons and assets in `src/assets/` as static SVG imports.
## Runtime
- **Node.js** — Required for development and build. No `.nvmrc` or explicit version constraint; `withastro/action@v3` defaults to Node 20 in CI. Minimum version implied by Astro 5.x requirements (Node >=18.14.1).
- **npm** — Lockfile `package-lock.json` present.
- No yarn, pnpm, or bun configuration detected.
## Frameworks
- **Astro 5.x** (`^5.7.5`) — Static site generator. Zero-JS-by-default output. All pages rendered at build time. Configured in `astro.config.mjs`.
- **Tailwind CSS v4** (`^4.1.4`) — Utility-first CSS framework. Integrated via the `@tailwindcss/vite` plugin (`astro.config.mjs` → `vite.plugins`), and imported as `@import "tailwindcss"` in `src/styles/global.css`. No Tailwind config file (`tailwind.config.*`) — v4 uses CSS-driven configuration.
- **Vite** — Bundled and managed by Astro internally. No standalone Vite config. The Tailwind Vite plugin is the only custom Vite plugin.
- **@tailwindcss/vite** (`^4.1.4`) — Vite plugin that processes Tailwind directives at build time.
## Key Dependencies
- `astro` ^5.7.5 — Core framework. Handles routing, component compilation, asset optimization, and static HTML output.
- `tailwindcss` ^4.1.4 — CSS utility framework. Provides the class system used across all components.
- `@tailwindcss/vite` ^4.1.4 — Build-time integration enabling Tailwind processing within Astro's Vite pipeline.
## Configuration
- `astro.config.mjs` — Sets `site` URL (`https://ggolov.github.io`) and registers the Tailwind Vite plugin.
- `tsconfig.json` — Extends `astro/tsconfigs/strict`. Includes `.astro/types.d.ts` and all source files. Excludes `dist/`.
- No `.env` files committed. `.env` and `.env.production` listed in `.gitignore`.
- Site URL is hardcoded in `astro.config.mjs` (not environment-driven).
## Missing / Notable Absences
- **No testing framework** — No Jest, Vitest, Playwright, or any test runner configured. No test files found.
- **No linter/formatter** — No ESLint, Prettier, or Biome configuration. No `.eslintrc.*`, `.prettierrc.*`, or `eslint.config.*` files.
- **No commit hooks** — No husky, lint-staged, or commitlint.
- **No CI lint/type-check step** — The deploy workflow (`deploy.yml`) only builds via `withastro/action@v3`. No separate type-check or lint job.
## Version Constraints
| Package | Version | Constraint |
|---------|---------|------------|
| astro | ^5.7.5 | Semver-compatible with 5.x |
| tailwindcss | ^4.1.4 | Semver-compatible with 4.x |
| @tailwindcss/vite | ^4.1.4 | Semver-compatible with 4.x |
<!-- GSD:stack-end -->

<!-- GSD:conventions-start source:CONVENTIONS.md -->
## Conventions

## Naming Patterns
- **Components** — PascalCase: `Home.astro`, `Header.astro`, `Footer.astro`, `Projects.astro`
- **Layouts** — PascalCase: `Layout.astro`
- **Pages** — `index.astro` (lowercase, uses file-based routing)
- **Static assets** — kebab-case: `linkedin-icon.svg`, `github-icon.svg`, `background.svg`, `favicon.svg`
- **Styles** — kebab-case: `global.css`
- **Config files** — kebab-case: `astro.config.mjs`, `.github/copilot-instructions.md`
- **Images from `src/assets/`** imported as Astro asset references, then accessed via `.src` property
- Functional, stateless components — no client-side props, no scripts, no interactivity
- No TypeScript types defined in components (all are plain Astro frontmatter with no explicit interface)
- Tailwind utility classes only (no custom CSS classes defined)
- Classes use Tailwind syntax: `flex`, `flex-row`, `p-3`, `font-bold`, `bg-amber-400`, etc.
- Custom CSS selectors: only the universal `* { transition-duration: 200ms; }` in `global.css`
## Component Structure
## Import Organization
## Asset Conventions
- SVG icons imported from `src/assets/` using standard Astro image import (`import icon from "../assets/icon.svg"`)
- Accessed via `.src` property: `{icon.src}`
- Favicon linked from `public/favicon.svg` in `Layout.astro`
- Inline attributes used: `width`, `height` on `<img>` tags
- No `alt` text on social icon images in `Home.astro` (`<img src={linkedinIcon.src} width="30" height="30" />`) — accessibility gap
## CSS Conventions
* {
- Tailwind CSS v4 via `@import "tailwindcss"` directive
- Single global CSS file imported in `Layout.astro`
- No component-scoped styles (empty `<style></style>` tags in all `.astro` components)
- No CSS variables, no custom properties defined
- All styling done inline via Tailwind utility classes
- Color palette: uses Tailwind's `amber` family (`amber-100`, `amber-400`, `amber-500`, `amber-600`)
## HTML Conventions
- `<header>`, `<footer>`, `<nav>`, `<main>`, `<aside>`, `<section>` — all used appropriately
- `<ol>` / `<li>` for ordered experience list
- `<ul>` / `<li>` for navigation and social links
- `<h1>` through `<h3>` for headings
- `<p>` for paragraphs
- `<span>` for inline text
- Standard `<!doctype html>` with `<html lang="en">`
- Viewport meta tag: `<meta name="viewport" content="width=device-width" />`
## Link Conventions
- `Footer.astro`: The "Astro" link has `target="_blank"` but **missing** `rel="noopener noreferrer"`
- `Footer.astro`: The "GitHub Pages" link has `target="_blank"` but **missing** `rel="noopener noreferrer"`
- `<a href="/">Home</a>`, `<a href="/projects">Projects</a>`, `<a href="/about">About</a>` in `Header.astro`
## Potential Issues / Convention Violations
### HTML Nesting Violation
### Placeholder Content
- `Home.astro`: Contains "Placeholder Role" and "Fourth Placeholder Role" entries with fake company names and dates
- `Projects.astro`: Entire component body is just the text "Projects"
### Empty `<style>` Tags
### Entity Encoding Issue
### Broken Link
### Missing Alt Text on Images
### Supabase Mention
### No TypeScript Interfaces
<!-- GSD:conventions-end -->

<!-- GSD:architecture-start source:ARCHITECTURE.md -->
## Architecture

## System Overview
```text
```
## Component Responsibilities
| Component | Responsibility | File |
|-----------|----------------|------|
| `Layout` | HTML shell (`<html>`, `<head>`, `<body>`), imports global CSS, renders `<slot />` | `src/layouts/Layout.astro` |
| `Header` | Site logo (GG), navigation bar (Home, Projects, About links) | `src/components/Header.astro` |
| `Footer` | Attribution text (Astro, Supabase, GitHub Pages) | `src/components/Footer.astro` |
| `Home` | Sidebar layout: identity card (name, title) + social links (LinkedIn, GitHub) + Experience timeline | `src/components/Home.astro` |
| `Projects` | Placeholder page content ("Projects" text) | `src/components/Projects.astro` |
| `index.astro` (home) | Composes `Layout` > `Header` + `Home` + `Footer` | `src/pages/index.astro` |
| `projects/index.astro` | Composes `Layout` > `Header` + `Projects` + `Footer` | `src/pages/projects/index.astro` |
## Pattern Overview
- **Fully static** — zero client-side JavaScript; all rendering happens at build time
- **Layout wrapping** — a shared `Layout.astro` shell wraps page content via Astro's `<slot />` mechanism
- **Page-section composition** — each page composes its sections (Header, content, Footer) within a Layout
- **Utility-first styling** — all visual design via Tailwind utility classes, no CSS modules or scoped CSS used
- **File-based routing** — Astro maps files in `src/pages/` to URL paths automatically
## Layers
- Purpose: Provides the HTML document shell, global styles, and structural container
- Location: `src/layouts/Layout.astro`
- Contains: HTML boilerplate, global CSS import, `<slot />` for child content, full-height flex column body
- Depends on: `src/styles/global.css`
- Used by: All page routes (`src/pages/`)
- Purpose: Defines routes and composes page-specific component arrangements
- Location: `src/pages/`
- Contains: `.astro` files representing URL routes; composes Layout + section components
- Depends on: Layout, components
- Used by: Build pipeline (Astro router)
- Purpose: Reusable UI sections representing logical page regions
- Location: `src/components/`
- Contains: Header, Footer, Home (sidebar + experience), Projects (placeholder)
- Depends on: Assets (`src/assets/`), Tailwind utilities
- Used by: Pages
- Purpose: Global style foundation
- Location: `src/styles/global.css`
- Contains: `@import "tailwindcss"` directive and global transition rule
- Depends on: Tailwind CSS v4, `@tailwindcss/vite` plugin
- Used by: Layout
- Purpose: Image and icon resources
- Location: `public/` and `src/assets/`
- Contains: favicon (`public/`), SVG icons (`src/assets/`)
- Used by: Components (via Astro asset imports)
## Data Flow
### Primary Request Path (Build Time)
### Build Pipeline Flow
- No state management — the site is fully static with zero client-side interactivity
- No JavaScript runtime, no frontend framework, no reactive state
## Key Abstractions
- Purpose: Provides a consistent document shell across all pages
- Examples: `src/layouts/Layout.astro`, `src/pages/index.astro`, `src/pages/projects/index.astro`
- Pattern: Astro `<slot />` composition — Layout defines HTML wrapper, pages pass content as children
- Purpose: Each page is composed of reusable section components rather than inline markup
- Examples: `index.astro` imports `Header`, `Home`, `Footer` separately
- Pattern: Astro component imports in frontmatter + template composition
- Purpose: SVG icons are imported as module references, using `.src` for the resolved path
- Examples: `src/components/Home.astro` imports `linkedinIcon` and `gitHubIcon` from `src/assets/`
- Pattern: `import icon from "../assets/icon.svg"` → `<img src={icon.src} />`
## Entry Points
- Location: `src/pages/index.astro`
- Triggers: Build-time route `/`
- Responsibilities: Composes the full homepage layout (Header + Home section + Footer)
- Location: `src/pages/projects/index.astro`
- Triggers: Build-time route `/projects`
- Responsibilities: Composes the projects page layout (Header + Projects section + Footer)
## Architectural Constraints
- **Static-only:** No server-side rendering, no API routes, no dynamic content — all content is baked at build time
- **No client JS:** Zero JavaScript runtime; the site is pure HTML + CSS
- **Single-threaded build:** Astro SSG runs on Node.js at build time; no server process
- **No global state:** No module-level state, no shared mutable data between components
- **No circular imports:** The dependency graph is strictly one-directional (Layout → Pages → Components → Assets)
## Anti-Patterns
### Mentioned-but-unconfigured service (Supabase)
### Placeholder/incomplete routes
### Placeholder content in production
## Error Handling
- Build errors surface during `astro build` with stack traces
- Missing routes result in 404 from GitHub Pages (no custom 404 page configured)
## Cross-Cutting Concerns
<!-- GSD:architecture-end -->

<!-- GSD:skills-start source:skills/ -->
## Project Skills

No project skills found. Add skills to any of: `.claude/skills/`, `.agents/skills/`, `.cursor/skills/`, `.github/skills/`, or `.codex/skills/` with a `SKILL.md` index file.
<!-- GSD:skills-end -->

<!-- GSD:workflow-start source:GSD defaults -->
## GSD Workflow Enforcement

Before using Edit, Write, or other file-changing tools, start work through a GSD command so planning artifacts and execution context stay in sync.

Use these entry points:
- `/gsd-quick` for small fixes, doc updates, and ad-hoc tasks
- `/gsd-debug` for investigation and bug fixing
- `/gsd-execute-phase` for planned phase work

Do not make direct repo edits outside a GSD workflow unless the user explicitly asks to bypass it.
<!-- GSD:workflow-end -->



<!-- GSD:profile-start -->
## Developer Profile

> Profile not yet configured. Run `/gsd-profile-user` to generate your developer profile.
> This section is managed by `generate-claude-profile` -- do not edit manually.
<!-- GSD:profile-end -->
