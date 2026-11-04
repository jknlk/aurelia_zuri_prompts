# Tech stack: Aurelia Zuri

| Layer | Choice | Version | Why |
| --- | --- | --- | --- |
| Build tool | [Vite](https://vitejs.dev) + `@vitejs/plugin-react` | 5.4.11 / 4.3.4 | Fast dev server, static build to `dist/` |
| UI | [React](https://react.dev) + React DOM | 18.3.1 | Components per chapter; state for loader, tour and menu |
| Animation | [GSAP](https://gsap.com) + ScrollTrigger | 3.13.0 | Scroll-scrubbed timelines, pinning, masked text reveals |
| Smooth scroll | [Lenis](https://lenis.darkroom.engineering) | 1.3.4 | Inertial scrolling, driven by GSAP's ticker so both stay on the same frame |
| Rendering | HTML `<canvas>` 2D | — | Draws the 300-frame tower film and tour clips with object-fit: cover |
| Vector art | Inline SVG | — | Loader house, footer mark, arch masks, soft polygon masks for room furnishing |
| Styling | Plain CSS with custom properties | — | `global.css` (tokens, loader, film, nav), `tour.css`, `chapters.css` |
| Fonts | Google Fonts: Instrument Serif, Inter | — | Editorial serif headlines, clean sans body |
| Media | JPG frame sequence, MP4 video, JPG photos | — | Served from `public/` |
| Tooling script | Node (`scripts/build-frames.mjs`) | — | Copies raw frames into `public/frames/f_001.jpg …` |

No backend, no CSS framework, no state library. The site builds to plain static files and can be hosted on any static host (Vercel, Netlify, S3 + CloudFront, GitHub Pages).

## Project layout

```
index.html                 fonts, favicon, preloads frame 1
src/
  main.jsx                 mounts <App/>, imports the three stylesheets
  App.jsx                  loader → film → chapters; owns load progress and scroll lock
  hooks/useLenis.js        Lenis on GSAP's ticker; locked until the loader is done
  lib/
    preloadFrames.js       two-pass frame loader (coarse pass, then fill in)
    canvas.js              drawCover / drawFrame (cover crop that eases to fit the tower)
    homeMark.js            house geometry shared by the loader and the footer
  config/
    site.js                brand, nav items, film beats, floor data, interior tour rooms
    chapters.js            copy, images and videos for every chapter
  components/
    Loader.jsx             SVG house build + arch-window reveal
    Experience.jsx         pinned canvas film and its text beats
    FloorTour.jsx          floor chooser, fly-in, room-to-room walkthrough
    RoomBuild.jsx          bare → furnished room via masked layers
    Nav.jsx                top bar and full-screen menu
    Chapters.jsx           chapter list, page-wide reveals, scroll rail, nav tone
    chapters/*.jsx         one component per chapter
  styles/
    global.css             tokens, loader, film, nav, menu
    tour.css               floor chooser and interiors
    chapters.css           chapters + the brand-accent block at the end
public/
  frames/                  tower film (300 JPGs)
  images/                  chapter photos (placeholders)
  tour/                    bare + furnished room photos (placeholders)
  vdos/                    wellness, water, sky terrace videos
```

## Scripts

```bash
npm install
npm run dev       # dev server on :5173
npm run build     # static build to dist/
npm run preview   # serve the build
npm run frames    # rebuild public/frames from "vdoframes 1st"
```

## Performance notes

- The film loads every 4th frame first (enough to scrub coarsely), unlocks the site, then fills in the rest in the background. The canvas always draws the nearest frame that has decoded.
- Videos use `preload="none"` with a poster image, and an IntersectionObserver plays or pauses them.
- Device pixel ratio is respected on the canvas; resizing re-measures ScrollTrigger.
- `prefers-reduced-motion` turns off smooth scrolling and turns tour moves into near-instant cuts.
