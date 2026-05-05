# WAAPI vs Motion

An interactive presentation exploring when to use the **Web Animations API** and when to reach for **Motion** (formerly Framer Motion).

Built for an R&D knowledge sharing session.

## [View the presentation](https://l-ubu.github.io/WAAPI-vs-Motion/)

## What's covered

- **WAAPI** — native browser animation API, zero dependencies, GPU-accelerated
- **Motion** — declarative library with springs, gestures, layout animations
- Live interactive demos for each concept (staggering, scroll-driven, drag, springs)
- Side-by-side comparison and decision framework

## Tech stack

| Layer | Tool |
|-------|------|
| Framework | SvelteKit + Svelte 5 |
| Presentation | Reveal.js |
| Styling | Tailwind CSS 4 |
| Code blocks | Shiki + Shiki Magic Move |
| Animations | @animotion/motion + Motion v12 |
| Deployment | GitHub Pages |

## Run locally

```bash
pnpm install
pnpm dev
```

## Controls

| Key | Action |
|-----|--------|
| Arrow keys | Navigate slides/fragments |
| `M` | Toggle dark/light mode |
| `F` | Fullscreen |
| `O` | Overview mode |

## License

MIT
