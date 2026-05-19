# Technology Stack

**Analysis Date:** 2026-05-19

## Languages

**Primary:**
- **TypeScript** — Strict mode, used throughout components and config. Config via `astro/tsconfigs/strict` in `tsconfig.json`.
- **Astro (`.astro` files)** — Component islands combining HTML, frontmatter TypeScript, and scoped CSS in a single file. Compiled at build time to static HTML.

**Secondary:**
- **CSS** — Tailwind utility classes in templates, plus one global CSS rule in `src/styles/global.css`.
- **SVG** — Icons and assets in `src/assets/` as static SVG imports.

## Runtime

**Environment:**
- **Node.js** — Required for development and build. No `.nvmrc` or explicit version constraint; `withastro/action@v3` defaults to Node 20 in CI. Minimum version implied by Astro 5.x requirements (Node >=18.14.1).

**Package Manager:**
- **npm** — Lockfile `package-lock.json` present.
- No yarn, pnpm, or bun configuration detected.

## Frameworks

**Core:**
- **Astro 5.x** (`^5.7.5`) — Static site generator. Zero-JS-by-default output. All pages rendered at build time. Configured in `astro.config.mjs`.

**Styling:**
- **Tailwind CSS v4** (`^4.1.4`) — Utility-first CSS framework. Integrated via the `@tailwindcss/vite` plugin (`astro.config.mjs` → `vite.plugins`), and imported as `@import "tailwindcss"` in `src/styles/global.css`. No Tailwind config file (`tailwind.config.*`) — v4 uses CSS-driven configuration.

**Build/Dev:**
- **Vite** — Bundled and managed by Astro internally. No standalone Vite config. The Tailwind Vite plugin is the only custom Vite plugin.
- **@tailwindcss/vite** (`^4.1.4`) — Vite plugin that processes Tailwind directives at build time.

## Key Dependencies

**Critical (runtime):**
- `astro` ^5.7.5 — Core framework. Handles routing, component compilation, asset optimization, and static HTML output.
- `tailwindcss` ^4.1.4 — CSS utility framework. Provides the class system used across all components.
- `@tailwindcss/vite` ^4.1.4 — Build-time integration enabling Tailwind processing within Astro's Vite pipeline.

## Configuration

**Build:**
- `astro.config.mjs` — Sets `site` URL (`https://ggolov.github.io`) and registers the Tailwind Vite plugin.
- `tsconfig.json` — Extends `astro/tsconfigs/strict`. Includes `.astro/types.d.ts` and all source files. Excludes `dist/`.

**Environment:**
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

All dependencies are pinned with caret (`^`) ranges, allowing minor and patch updates automatically.

---

*Stack analysis: 2026-05-19*
