# Codebase Structure

**Analysis Date:** 2026-05-19

## Directory Layout

```
ggolov.github.io/
├── .github/
│   ├── copilot-instructions.md   # GitHub Copilot agent instructions
│   └── workflows/
│       └── deploy.yml            # GitHub Pages deployment workflow
├── .planning/                    # GSD planning artifacts (auto-generated)
│   └── codebase/
│       ├── ARCHITECTURE.md       # This file
│       └── STRUCTURE.md          # This file
├── openspec/
│   ├── config.yaml               # OpenSpec schema config
│   ├── changes/                  # Change proposals
│   │   └── archive/              # Archived / completed changes
│   └── specs/                    # Specification documents
├── public/
│   └── favicon.svg               # Site favicon
├── src/
│   ├── assets/
│   │   ├── astro.svg             # Astro logo asset
│   │   ├── background.svg        # Background image asset
│   │   ├── github-icon.svg       # GitHub social icon
│   │   └── linkedin-icon.svg     # LinkedIn social icon
│   ├── components/
│   │   ├── Footer.astro          # Page footer — attribution links
│   │   ├── Header.astro          # Site header — logo + navigation
│   │   ├── Home.astro            # Homepage main content — sidebar + experience
│   │   └── Projects.astro        # Projects page content (placeholder)
│   ├── layouts/
│   │   └── Layout.astro          # HTML document shell with <slot />
│   ├── pages/
│   │   ├── index.astro           # Homepage route (/)
│   │   └── projects/
│   │       └── index.astro       # Projects route (/projects)
│   └── styles/
│       └── global.css            # Tailwind CSS import + global styles
├── AGENTS.md                     # AI agent guidance document
├── README.md                     # Default Astro starter README
├── astro.config.mjs              # Astro framework configuration
├── package.json                  # Project manifest + dependencies
└── tsconfig.json                 # TypeScript configuration
```

## Directory Purposes

**`.github/workflows/`:**
- Purpose: GitHub Actions CI/CD configuration
- Contains: YAML pipeline definitions
- Key files: `deploy.yml` — Builds with `withastro/action@v3` and deploys to GitHub Pages

**`public/`:**
- Purpose: Static files served as-is at the site root (no Astro processing)
- Contains: Root-level assets like favicon
- Convention: Files here map to URL paths directly (e.g., `favicon.svg` → `/favicon.svg`)

**`src/assets/`:**
- Purpose: Importable static assets processed by Astro/Vite (supports asset hashing and optimization)
- Contains: SVG icons and images used by components
- Convention: Import with JavaScript import syntax and use `.src` for resolved URL

**`src/components/`:**
- Purpose: Reusable Astro components representing page sections
- Contains: `.astro` component files, one per logical UI section
- Key files:
  - `Header.astro` — Navigation bar (logo + links)
  - `Footer.astro` — Attribution bar
  - `Home.astro` — Main homepage content (sidebar + experience list)
  - `Projects.astro` — Placeholder projects content

**`src/layouts/`:**
- Purpose: Page-level layout shells
- Contains: `.astro` layout wrappers
- Key files: `Layout.astro` — Single shared layout with full-height flex column body

**`src/pages/`:**
- Purpose: Route definitions — Astro creates a page for every `.astro` file
- Contains: Page-level `.astro` files that compose Layout + Components
- Key files:
  - `index.astro` — Route `/`
  - `projects/index.astro` — Route `/projects`
- Convention: Files map to URL paths; directories create nested routes

**`src/styles/`:**
- Purpose: Global stylesheets
- Contains: CSS files imported by layouts/pages
- Key files: `global.css` — Tailwind v4 import and global transition rule

**`openspec/`:**
- Purpose: OpenSpec-driven specification and change management
- Contains: YAML config, change proposals (active + archived), specification documents
- Status: Configured (`config.yaml`) but no specs yet (`specs/` directory is empty)

## Key File Locations

**Entry Points:**
- `src/pages/index.astro`: Homepage route — primary site entry point
- `src/pages/projects/index.astro`: Projects page route

**Configuration:**
- `astro.config.mjs`: Astro framework configuration (site URL, Vite plugins)
- `tsconfig.json`: TypeScript strict mode configuration
- `package.json`: Project metadata, scripts, and dependencies
- `.github/workflows/deploy.yml`: CI/CD pipeline for GitHub Pages

**Core Logic:**
- `src/layouts/Layout.astro`: Application shell and HTML structure
- `src/components/Header.astro`: Navigation component
- `src/components/Home.astro`: Homepage main content with sidebar and experience list
- `src/components/Projects.astro`: Projects page content (placeholder)

**Styling:**
- `src/styles/global.css`: Tailwind CSS v4 import and global styles

**Static Assets:**
- `public/favicon.svg`: Site favicon
- `src/assets/*.svg`: Icons and images used by components

## Naming Conventions

**Files:**
- **Astro components:** PascalCase — `Header.astro`, `Footer.astro`, `Home.astro`, `Projects.astro`, `Layout.astro`
- **Pages:** lowercase with index files for nested routes — `index.astro`, `projects/index.astro`
- **CSS:** lowercase with dashes — `global.css`
- **Config:** kebab-case — `astro.config.mjs`, `tsconfig.json`

**Directories:**
- **Top-level:** lowercase — `src/`, `public/`, `openspec/`
- **Within src:** lowercase plural nouns — `components/`, `layouts/`, `pages/`, `styles/`, `assets/`

## Where to Add New Code

**New Page/Route:**
- Create file in `src/pages/` (e.g., `src/pages/about.astro` for `/about`)
- Compose with Layout + Header + Footer (see `src/pages/index.astro` as template)
- Add nav link in `src/components/Header.astro`

**New Component:**
- Add file to `src/components/` using PascalCase naming (e.g., `About.astro`)
- Import assets from `src/assets/` if needed

**New Static Asset:**
- Use `src/assets/` for assets referenced by components (processed by Vite)
- Use `public/` for assets served at root URL (e.g., PDFs, other favicons)

**New Global Style:**
- Add to `src/styles/global.css` — currently only contains the Tailwind import and a universal transition rule

**New Configuration:**
- Astro config: `astro.config.mjs`
- TypeScript: `tsconfig.json`
- Deployment: `.github/workflows/deploy.yml`

## Structural Gaps & Inconsistencies

1. **Missing About page:** `src/components/Header.astro` links to `/about`, but no corresponding route exists in `src/pages/`. Visiting `/about` will result in a 404 (or GitHub Pages default 404 page).

2. **No custom 404 page:** Astro supports `src/pages/404.astro` for custom error pages, but none is configured. Visitors hitting missing URLs see the platform default.

3. **Supabase mentioned but absent:** `src/components/Footer.astro` credits Supabase, but no Supabase integration exists in the project (no package, no config, no API calls).

4. **Placeholder content:** `src/components/Projects.astro` is a bare "Projects" text — clearly a stub. `src/components/Home.astro` contains two placeholder experience entries with fake data.

5. **Empty `openspec/specs/`:** The OpenSpec directory structure exists but has no spec documents yet. The `openspec/changes/` directory exists (with an `archive/` subdirectory) but is empty.

6. **`src/assets/astro.svg` and `src/assets/background.svg`:** These files exist but are not referenced by any component. They may be unused remnants from scaffolding.

7. **`README.md` is default Astro starter:** Does not describe the actual project or its purpose. Should be updated to reflect the personal website.

---

*Structure analysis: 2026-05-19*
