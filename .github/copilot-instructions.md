# GitHub Copilot Instructions

This repository is a simple static personal website built with Astro and Tailwind CSS.

## Key guidance for AI agents
- Prefer editing existing Astro pages and components over adding large new systems.
- Keep the site static unless the task explicitly requests interactivity or new client-side behavior.
- Preserve Astro frontmatter syntax and import assets at the top of `.astro` files.
- Use `target="_blank" rel="noopener noreferrer"` for external links.
- Avoid adding backend APIs, server routes, or full-stack features unless the repo is extended with them.

## Important files
- `src/pages/index.astro`
- `src/layouts/Layout.astro`
- `src/components/Home.astro`
- `src/components/Header.astro`
- `src/components/Footer.astro`
- `src/styles/global.css`
- `astro.config.mjs`

## Additional context
- This project uses Tailwind CSS via `@import "tailwindcss"` in `src/styles/global.css`.
- The current `README.md` is the default Astro starter README; do not assume it describes project-specific requirements.
- No tests or CI conventions are defined in this repository yet.

For more details, see `AGENTS.md` at the repository root.
