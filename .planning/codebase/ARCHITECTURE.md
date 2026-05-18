<!-- refreshed: 2026-05-19 -->
# Architecture

**Analysis Date:** 2026-05-19

## System Overview

```text
┌─────────────────────────────────────────────────────────────────┐
│                      Browser (HTTP Request)                      │
└──────────────────────────┬──────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Astro SSG Build Pipeline                      │
│              (astro build → static HTML/CSS output)              │
├──────────────────┬──────────────────┬───────────────────────────┤
│   Layout.astro   │   Page Routes    │    Components             │
│   (HTML shell)   │  (file-based)    │   (reusable sections)     │
│  `layouts/`      │  `pages/`        │   `components/`           │
└────────┬─────────┴────────┬─────────┴──────────┬────────────────┘
         │                  │                     │
         ▼                  ▼                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Tailwind CSS v4 Layer                         │
│        global.css (@import "tailwindcss") + utility classes      │
└─────────────────────────────────────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Static Output (dist/)                         │
│          Deployed to GitHub Pages via .github/workflows          │
└─────────────────────────────────────────────────────────────────┘
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

**Overall:** Static Site Generation (SSG) via Astro with component-based composition and utility-first styling.

**Key Characteristics:**
- **Fully static** — zero client-side JavaScript; all rendering happens at build time
- **Layout wrapping** — a shared `Layout.astro` shell wraps page content via Astro's `<slot />` mechanism
- **Page-section composition** — each page composes its sections (Header, content, Footer) within a Layout
- **Utility-first styling** — all visual design via Tailwind utility classes, no CSS modules or scoped CSS used
- **File-based routing** — Astro maps files in `src/pages/` to URL paths automatically

## Layers

**Layout Layer:**
- Purpose: Provides the HTML document shell, global styles, and structural container
- Location: `src/layouts/Layout.astro`
- Contains: HTML boilerplate, global CSS import, `<slot />` for child content, full-height flex column body
- Depends on: `src/styles/global.css`
- Used by: All page routes (`src/pages/`)

**Page Layer:**
- Purpose: Defines routes and composes page-specific component arrangements
- Location: `src/pages/`
- Contains: `.astro` files representing URL routes; composes Layout + section components
- Depends on: Layout, components
- Used by: Build pipeline (Astro router)

**Component Layer:**
- Purpose: Reusable UI sections representing logical page regions
- Location: `src/components/`
- Contains: Header, Footer, Home (sidebar + experience), Projects (placeholder)
- Depends on: Assets (`src/assets/`), Tailwind utilities
- Used by: Pages

**Styling Layer:**
- Purpose: Global style foundation
- Location: `src/styles/global.css`
- Contains: `@import "tailwindcss"` directive and global transition rule
- Depends on: Tailwind CSS v4, `@tailwindcss/vite` plugin
- Used by: Layout

**Static Assets Layer:**
- Purpose: Image and icon resources
- Location: `public/` and `src/assets/`
- Contains: favicon (`public/`), SVG icons (`src/assets/`)
- Used by: Components (via Astro asset imports)

## Data Flow

### Primary Request Path (Build Time)

1. **Route resolution** — Astro scans `src/pages/` for `.astro` files and maps them to URL paths (`src/pages/index.astro` → `/`, `src/pages/projects/index.astro` → `/projects`)
2. **Page rendering** — Each page file renders its component tree:
   - `index.astro` renders `<Layout>` wrapper → `<Header />` + `<Home />` + `<Footer />`
   - `projects/index.astro` renders `<Layout>` wrapper → `<Header />` + `<Projects />` + `<Footer />`
3. **Style extraction** — Tailwind scans the rendered HTML for utility class usage and generates only the CSS classes actually used (v4 JIT engine)
4. **Static output** — Astro writes fully self-contained HTML files to `dist/`, with inlined/imported CSS

### Build Pipeline Flow

1. `npm run build` → `astro build`
2. `@tailwindcss/vite` plugin processes Tailwind directives during Vite bundling
3. Astro SSG generates static HTML pages from `.astro` files
4. Output written to `dist/`
5. GitHub Actions workflow (`.github/workflows/deploy.yml`) uploads `dist/` to GitHub Pages

**State Management:**
- No state management — the site is fully static with zero client-side interactivity
- No JavaScript runtime, no frontend framework, no reactive state

## Key Abstractions

**Layout/Slot Pattern:**
- Purpose: Provides a consistent document shell across all pages
- Examples: `src/layouts/Layout.astro`, `src/pages/index.astro`, `src/pages/projects/index.astro`
- Pattern: Astro `<slot />` composition — Layout defines HTML wrapper, pages pass content as children

**Page-Section Composition:**
- Purpose: Each page is composed of reusable section components rather than inline markup
- Examples: `index.astro` imports `Header`, `Home`, `Footer` separately
- Pattern: Astro component imports in frontmatter + template composition

**Asset Import via Astro:**
- Purpose: SVG icons are imported as module references, using `.src` for the resolved path
- Examples: `src/components/Home.astro` imports `linkedinIcon` and `gitHubIcon` from `src/assets/`
- Pattern: `import icon from "../assets/icon.svg"` → `<img src={icon.src} />`

## Entry Points

**Homepage:**
- Location: `src/pages/index.astro`
- Triggers: Build-time route `/`
- Responsibilities: Composes the full homepage layout (Header + Home section + Footer)

**Projects Page:**
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

**What happens:** The Footer.astro component links to "Supabase" as a technology credit, but Supabase is not installed, configured, or used anywhere in the project (`src/components/Footer.astro` line 10).
**Why it's wrong:** Misleading attribution; suggests a dependency that doesn't exist. A user reading the footer might assume the site uses Supabase for something.
**Do this instead:** Either remove the Supabase mention or implement it if it's intended. See `src/components/Footer.astro` for the source.

### Placeholder/incomplete routes

**What happens:** The Header nav includes an "About" link pointing to `/about`, but no corresponding page exists at `src/pages/about.astro` or `src/pages/about/index.astro` (`src/components/Header.astro` line 14).
**Why it's wrong:** Users clicking "About" will get a 404 (or the Astro SPA-style fallback). This is an unfinished navigation item.
**Do this instead:** Create the About page or remove the link.

### Placeholder content in production

**What happens:** Home.astro includes two placeholder experience entries ("Placeholder Role", "Fourth Placeholder Role") with obviously fake data (`src/components/Home.astro` lines 50-68). Projects.astro is a single "Projects" text placeholder (`src/components/Projects.astro` line 4).
**Why it's wrong:** Real content should replace placeholders before public deployment.
**Do this instead:** Replace with actual experience entries and project content.

## Error Handling

**Strategy:** No explicit error handling — the site is fully static. Astro's SSG catches build-time errors during `astro build`. Runtime errors are not applicable due to zero client JavaScript.

**Patterns:**
- Build errors surface during `astro build` with stack traces
- Missing routes result in 404 from GitHub Pages (no custom 404 page configured)

## Cross-Cutting Concerns

**Logging:** None — no server process, no client-side logging.
**Validation:** None — no forms, no user input, no API interactions.
**Authentication:** None — public site, no authenticated features.

---

*Architecture analysis: 2026-05-19*
