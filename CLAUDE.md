# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pnpm dev          # Start dev server (Vite + SvelteKit)
pnpm build        # Production build
pnpm check        # Type-check (svelte-kit sync + svelte-check)
```

No test suite or linter is configured.

## Architecture

This is a **slide deck** built with SvelteKit + Reveal.js, using a custom component library (in `src/lib/`) inspired by [Animotion](https://animotion.dev). The presentation topic is "Web Animations: WAAPI vs Motion".

### Slide system

Slides live in `src/slides/<number>/slide.svelte`, numbered in increments of 100. The `Slides` component (`src/lib/components/slides.svelte`) auto-discovers them via `import.meta.glob` and sorts by number. Slides can export `props` (via `defineProps`) to configure per-slide Reveal.js options like `transition` or `animate`.

### Key components (`src/lib/components/`)

- **Presentation** — initializes Reveal.js, wires custom events (`in`, `out`, `current`) for fragment-based animations
- **Slide** — wraps a `<section>` with Reveal.js data attributes (auto-animate, backgrounds, transitions)
- **Action** — invisible fragment that fires `do`/`undo` callbacks on navigation (forward/backward)
- **Transition** — fragment with View Transitions API integration; supports named transitions, custom entry/exit animations
- **Code** — Shiki Magic Move powered code block with `update`, `selectLines`, `selectToken`, `scrollToLine` tagged-template APIs

### Animation approach

Slides use `@animotion/motion` tweens and the `Action` component to orchestrate step-by-step animations triggered by Reveal.js fragment navigation. The `Code` component provides animated code transitions via Shiki Magic Move.

### Styling

- Tailwind CSS 4 (via `@tailwindcss/vite` plugin)
- Custom CSS variables for palette in `src/styles/app.css` (dark default, light toggle via `M` key)
- Reveal.js theme overrides in `src/lib/styles/theme.css`
- Fonts: Inter Variable (body), Monaspace Neon (code)
