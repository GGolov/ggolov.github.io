# Testing Patterns

**Analysis Date:** 2026-05-19

## Test Framework

**Status: NONE**

No test runner, assertion library, or testing framework is configured in the project. The `package.json` contains no `test` script and no test-related dependencies. There are no test files anywhere in the repository (`*.test.*`, `*.spec.*` — none found).

**Run Commands:**
```bash
# No test commands exist. Available scripts from package.json:
npm run dev       # astro dev
npm run build     # astro build
npm run preview   # astro preview
npm run astro     # astro CLI
```

## Test File Organization

**Status: N/A** — No test files exist.

**Expected location (if added):**
- Co-located with source: `src/components/Home.test.ts` or `src/components/Home.test.astro`
- Or placed in a top-level `tests/` directory

## Current Coverage

**Overall: 0%**

There are zero tests covering any of the following:

| Component/Module | File | What Should Be Tested |
|------------------|------|-----------------------|
| Layout | `src/layouts/Layout.astro` | Correct HTML shell rendering, meta tags, slot behavior |
| Header | `src/components/Header.astro` | Nav links render, active state, correct href values |
| Home | `src/components/Home.astro` | Experience list renders, social links present, sidebar renders |
| Footer | `src/components/Footer.astro` | Attribution text renders, external links have correct attributes |
| Projects | `src/components/Projects.astro` | Placeholder content renders (minimal) |
| Page composition | `src/pages/index.astro` | All components compose correctly |
| Page composition | `src/pages/projects/index.astro` | All components compose correctly |
| Config | `astro.config.mjs` | Site URL, plugin config |
| Global styles | `src/styles/global.css` | Tailwind import, transition rule |

## What Should Be Tested

### Unit Tests (Component Rendering)
- Each `.astro` component renders without errors
- `Header.astro` renders 3 nav links: Home, Projects, About
- `Header.astro` logo link points to `/`
- `Footer.astro` contains expected text: "Astro", "Supabase", "GitHub Pages"
- `Footer.astro` external links use `target="_blank"` with `rel="noopener noreferrer"`
- `Home.astro` renders experience list items
- `Home.astro` renders social links (LinkedIn, GitHub)
- `Projects.astro` renders placeholder content

### Link Validity Tests
- All internal links (`/`, `/projects`, `/about`) resolve to existing routes
- External links point to valid URLs

### HTML Validation
- No invalid HTML nesting (e.g., the nested `<li>` in `Home.astro`)
- Semantic elements used correctly

### Accessibility Checks
- Images have `alt` attributes
- Heading hierarchy is logical
- Color contrast meets WCAG standards

### Build Checks
- `astro build` completes without errors
- All page routes produce expected output files
- No missing page routes for linked pages (e.g., `/about`)

## Recommended Testing Approach

### Recommended Stack

| Tool | Purpose | Package |
|------|---------|---------|
| **Vitest** | Test runner | `vitest` |
| **@astrojs/check** | Type checking + diagnostics | `@astrojs/check` + `typescript` |
| **@testing-library/astro** | Component rendering assertions | `@testing-library/astro` |
| **happy-dom** or **jsdom** | DOM environment for tests | `happy-dom` |

### Installation
```bash
npm install -D vitest @testing-library/astro happy-dom @astrojs/check
```

### Recommended Config

**`vitest.config.ts`:**
```typescript
import { defineConfig } from 'vitest/config';
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
  plugins: [tailwindcss()],
  test: {
    environment: 'happy-dom',
    include: ['src/**/*.test.{ts,astro}'],
  },
});
```

**Recommended `package.json` scripts addition:**
```json
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest",
    "test:coverage": "vitest run --coverage",
    "check": "astro check"
  }
}
```

### Recommended Test Structure

**Component test example** (`src/components/Header.test.ts`):
```typescript
import { describe, it, expect } from 'vitest';
import { render } from '@testing-library/astro';
import Header from './Header.astro';

describe('Header', () => {
  it('renders all navigation links', async () => {
    const { container } = await render(Header);
    const links = container.querySelectorAll('nav a');
    expect(links).toHaveLength(3);
    expect(links[0]).toHaveAttribute('href', '/');
    expect(links[1]).toHaveAttribute('href', '/projects');
    expect(links[2]).toHaveAttribute('href', '/about');
  });

  it('renders logo link to home', async () => {
    const { container } = await render(Header);
    const logo = container.querySelector('header a');
    expect(logo).toHaveAttribute('href', '/');
  });
});
```

### Type Checking
Install and run `@astrojs/check` to validate Astro files:
```bash
npx @astrojs/check
```

Add a `check` script to `package.json` and include it in CI.

## CI Pipeline Gaps

Current CI (`.github/workflows/deploy.yml`) does **only** build + deploy:

| Step | Present? | Notes |
|------|----------|-------|
| Install dependencies | ✅ (via `withastro/action`) | |
| Build | ✅ (via `withastro/action`) | |
| Deploy | ✅ (`actions/deploy-pages@v4`) | |
| Type checking | ❌ | Not present |
| Lint | ❌ | Not present (no linter configured) |
| Tests | ❌ | Not present (no test framework) |
| Accessibility audit | ❌ | Not present |

### Recommended CI Additions
Add these steps before the build step in `.github/workflows/deploy.yml`:
```yaml
- name: Type check
  run: npx astro check

- name: Run tests
  run: npm test
```

## Additional Recommendations

### Linting
No linter is configured (no eslint, prettier, biome). Recommendations:
- **Install `biome`** (modern, fast, single tool for lint + format):
  ```bash
  npm install -D @biomejs/biome
  npx biome init
  ```
- Or install **ESLint** with `@eslint/json` and `eslint-plugin-astro`

### Pre-commit Hooks
Consider `husky` + `lint-staged` to run type checking, linting, and tests before every commit.

### Coverage Target
For a small static site, aim for:
- **95%+** component rendering coverage
- **100%** link validity coverage
- **100%** build success guarantee via CI

---

*Testing analysis: 2026-05-19*
