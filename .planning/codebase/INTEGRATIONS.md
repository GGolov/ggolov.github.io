# External Integrations

**Analysis Date:** 2026-05-19

## Hosting & Deployment

**GitHub Pages:**
- **URL:** `https://ggolov.github.io`
- **Provider:** GitHub Pages (free tier, static site hosting)
- **Config:** Site URL set in `astro.config.mjs` via `site: 'https://ggolov.github.io'`
- **Deployment:** Automatic via GitHub Actions workflow (`.github/workflows/deploy.yml`)
- **Custom domain:** Not configured (uses default `*.github.io` subdomain)

## CI/CD

**GitHub Actions:**
- **Trigger:** Push to `main` branch OR manual `workflow_dispatch`
- **Permissions:** `contents: read`, `pages: write`, `id-token: write`
- **Build step:** Uses `withastro/action@v3` — official Astro GitHub Action that installs dependencies, runs `astro build`, and uploads the `dist/` folder as a Pages artifact
- **Deploy step:** Uses `actions/deploy-pages@v4` — deploys the uploaded artifact to GitHub Pages
- **Node version:** Defaults to Node 20 (handled by `withastro/action@v3`; not explicitly overridden in workflow)
- **Package manager:** Auto-detected by `withastro/action@v3` from lockfile (`package-lock.json` → npm)

## Third-Party Assets

**SVG Icons (local, self-hosted):**
| File | Source/Type |
|------|-------------|
| `src/assets/astro.svg` | Astro logo (local copy) |
| `src/assets/background.svg` | Background graphic (local copy) |
| `src/assets/github-icon.svg` | GitHub icon (local copy) |
| `src/assets/linkedin-icon.svg` | LinkedIn icon (local copy) |
| `public/favicon.svg` | Site favicon |

All icons are bundled as local static assets — no external CDN or icon library dependency.

## External Links (User-Facing)

| Link | Target URL | Location |
|------|------------|----------|
| LinkedIn Profile | `https://www.linkedin.com/in/gleb-golov/` | `src/components/Home.astro` (Socials sidebar) |
| GitHub Profile | `https://github.com/GGolov` | `src/components/Home.astro` (Socials sidebar) |
| Astro | `https://astro.build/` | `src/components/Footer.astro` |
| Supabase | `https://supabase.com/` | `src/components/Footer.astro` |
| GitHub Pages | `https://pages.github.com/` | `src/components/Footer.astro` |

All external links use `target="_blank" rel="noopener noreferrer"`.

## Supabase — Flagged (Ghost Reference)

**⚠️ Supabase is referenced in the footer (`src/components/Footer.astro`) but is NOT actually integrated:**

- **No SDK dependency** — `@supabase/supabase-js` is not in `package.json`
- **No environment variables** — `SUPABASE_URL`, `SUPABASE_ANON_KEY` etc. are not configured
- **No client code** — No Supabase imports, queries, or authentication anywhere in the codebase
- **No server routes** — No Astro API endpoints or server-side Supabase calls
- **Status:** This is a cosmetic/aspirational reference in the footer text. Supabase provides no functionality in the current site.

## OpenSpec

- **File:** `openspec/config.yaml`
- **Schema:** `spec-driven` — OpenSpec configuration present but no specs or changes have been created yet.
- **Type:** Project-scoped specification tool (not an external service).
- **Status:** Scaffolded but empty. No functional impact on the site.

## Other Integrations

- **No analytics** — No Google Analytics, Plausible, Umami, or other tracking.
- **No form service** — No contact forms or form handlers.
- **No comment system** — No Disqus, Giscus, or similar.
- **No newsletter/CMS** — No Mailchimp, Buttondown, Contentful, etc.
- **No monitoring/error tracking** — No Sentry, LogRocket, etc.

---

*Integration audit: 2026-05-19*
