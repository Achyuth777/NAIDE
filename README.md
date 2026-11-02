# NAÏADE — The Deep End

A storefront for a fictional fine-jewellery house, built as one continuous camera move driven by the scrollbar.

The page opens at night on a woman seated at the edge of a lit pool, wearing Paraíba-blue stones. As you scroll, the camera cranes up to her face, then moves in on the earring, down to the pendant and down to the ring. Each time it lands on a piece, a product card slides in with specs, price and an add-to-bag button. The film then rewinds to the wide shot. An ivory section holding all three pieces and the full set rises over it, and the page ends on a large wordmark and a private-viewing form.

> NAÏADE is a concept brand. The pieces, prices and imagery are fictional, and checkout is not connected.

## Tech stack

- Vite 6, React 19, TypeScript
- Tailwind CSS v4 (via `@tailwindcss/vite`, theme in `src/index.css`)
- Lenis for smooth scrolling
- No animation library: every scroll beat is plain arithmetic on scroll progress

## Getting started

```bash
npm install
npm run dev
```

| Command           | What it does                    |
| ----------------- | ------------------------------- |
| `npm run dev`     | Start the dev server            |
| `npm run build`   | Build for production to `dist/` |
| `npm run preview` | Serve the production build      |
| `npm run lint`    | Type-check with `tsc --noEmit`  |

## How it works

- **One animation loop.** `src/lib/scroll.ts` advances Lenis, then each scroll-driven component reads its position and writes styles directly. Nothing goes through React state while scrolling.
- **The timeline is the page.** `src/lib/timeline.ts` defines the camera move as windows of scroll progress: the hero, the three product holds and the rewind. Changing one window shifts every beat after it.
- **Speed ramping.** `src/lib/timeRemap.ts` maps scroll progress to film time with a monotone cubic curve, so holds are slow and travelling shots are fast, and the film never runs backwards while you scroll forwards.
- **Frames, not video.** The film is 481 WebP frames (`public/frames/d` for desktop, `public/frames/m` for phones), drawn to a canvas. Picking a frame is an array lookup, so fast scrolling never drops seeks.
- **Cards share the screen.** On desktop the film narrows beside each card so the jewel stays in view. On phones the card comes up from the bottom.

## Project structure

```
public/
  frames/d, frames/m   film frames (desktop / phone)
  media/               product images
src/
  components/          Stage, ProductCard, Nav, Closing, Footer, BagDrawer, …
  hooks/               reveal, fit-text and reduced-motion hooks
  lib/                 scroll loop, timeline, frame loader, bag store, math
  content.ts           all copy, products and prices
```

To change products, copy or prices, edit `src/content.ts`.
