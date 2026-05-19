# AGENTS

## Purpose
This repository is a small personal website built with Astro and Tailwind CSS. The instructions here help AI coding agents make safe, focused changes without introducing unrelated complexity.

## Project summary
- Framework: Astro 5.x
- Styling: Tailwind CSS v4 plugin via `@import "tailwindcss"` in `src/styles/global.css`
- Source content is static and page-oriented
- Main entrypoint: `src/pages/index.astro`
- Layout and components are in `src/layouts/` and `src/components/`
- Static assets belong in `public/` and `src/assets/`

## Common commands
Run from the repository root.
- `npm install` - install dependencies
- `npm run dev` - start local development server
- `npm run build` - produce production output
- `npm run preview` - preview built site

## Agent guidance
- Prefer editing existing Astro pages and components over creating large new systems.
- Keep the site simple and static unless a request explicitly asks for interactivity.
- Preserve existing Astro syntax: use frontmatter `---`, import assets at the top, and use standard Astro component composition.
- Keep external links secure: `target="_blank" rel="noopener noreferrer"`.
- Avoid adding backend APIs, server routes, or full-stack features unless the repo is extended with them.

## Key files
- `src/pages/index.astro` - homepage composition
- `src/layouts/Layout.astro` - page shell and global HTML structure
- `src/components/Home.astro`, `Header.astro`, `Footer.astro` - page sections
- `src/styles/global.css` - Tailwind import and global CSS
- `astro.config.mjs` - Astro project configuration

## Notes
- The current README is the default Astro starter README; do not assume it reflects any extra project-specific requirements.
- There are no tests or CI conventions defined in this repository yet.
