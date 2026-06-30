# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev          # Start dev server (http://localhost:4321)
npm run build        # Build for production (also runs pagefind indexing via postbuild)
npm run preview      # Preview production build
npm run check        # Type-check with astro check + biome lint
npm run lint         # Biome lint with auto-fix
npm run format       # Prettier formatting
```

There are no tests in this project.

## Architecture

This is an **Astro 6** personal portfolio/blog using the Cactus theme, with Tailwind CSS v4 and MDX.

### Content Collections (`src/content/`)

- **`post/`** — Blog posts (`.md` / `.mdx`). Required frontmatter: `title`, `description`, `publishDate`, `tags`. Optional: `draft`, `coverImage`, `ogImage`, `updatedDate`, `pinned`.
- **`note/`** — Short-form notes. Simpler schema: `title`, `publishDate` (ISO 8601 with offset required).
- **`tag/`** — Optional tag metadata (title, description) to enrich tag pages.

Schema is defined in `src/content.config.ts`. Title is capped at 60 chars.

### Site Configuration (`src/site.config.ts`)

Single source of truth for site URL, title, author, locale/date settings, nav links, and Expressive Code theme options. Update this file to change global metadata.

### OG Image Generation (`src/pages/og-image/`)

Dynamic PNG generation per post slug using Satori + `sharp`. Fonts are loaded from `src/assets/` via a custom Vite plugin in `astro.config.ts` (`rawFonts`).

### Custom Remark Plugins (`src/plugins/`)

- **`remark-reading-time`** — Injects `minutesRead` into frontmatter data.
- **`remark-admonitions`** — Converts `:::note`, `:::tip`, `:::warning`, `:::caution`, `:::important` directive blocks into styled callouts.
- **`remark-github-card`** — Renders GitHub repo cards from a custom directive syntax.

### Search

Full-text search via **Pagefind**, which indexes the `dist/` folder after build. The `npm run build` script runs `pagefind --site dist` automatically. Search UI is in `src/components/Search.astro`.

### Styling

- Tailwind CSS v4 (configured via `@tailwindcss/vite` Vite plugin, not PostCSS)
- Global styles in `src/styles/global.css`; component-specific styles in `src/styles/blocks/` and `src/styles/components/`
- Dark/light theme toggled via `data-theme` attribute on `<html>`; theme logic lives in `src/components/ThemeProvider.astro`

### Linting & Formatting

- **Biome** for TS/JS/Astro linting and formatting (tabs, 100-char lines, trailing commas)
- **Prettier** with `prettier-plugin-astro` and `prettier-plugin-tailwindcss` for `.astro` files
- Astro files have relaxed Biome rules (unused vars/imports not flagged, `useConst` off)
