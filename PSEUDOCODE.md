# Pseudocode: Aurelia Zuri

How the site works, module by module. Names match the source in [jknlk/aurelia_zuri](https://github.com/jknlk/aurelia_zuri).

## 1. App: loader → film → chapters

```
App:
  progress   = 0        # real load progress 0 → 1
  seq        = null     # frame images once the coarse pass is ready
  revealed   = false    # loader has opened the arch
  done       = false    # loader removed, scrolling unlocked

  lenis = useLenis(enabled = done)

  on mount:
    job = preloadFrames(count = 300, path = "/frames/f_NNN.jpg",
                        onProgress = p -> progress = p)
    when job.ready: seq = { images, loaded }

  render:
    grain overlay
    if seq:
      Nav(revealed)
      Experience(images, loaded, revealed)   # the film + floor chooser
      Chapters()
    if not done:
      Loader(progress, onReveal -> revealed = true, onDone -> done = true)
```

## 2. Two-pass frame preloader

```
preloadFrames(count, path, stride = 4, concurrency = 12):
  coarse = every stride-th index, plus the last
  fine   = all the others
  loaded = byte array [count]

  run(queue):
    start `concurrency` workers
    each worker: take next index → new Image(path(i)) → decode()
                 on success loaded[i] = 1   (failures never block)
                 report finished / queue.length

  ready = run(coarse) with onProgress
  all   = ready then run(fine)
  return { images, loaded, ready, all }

nearestLoaded(loaded, i):
  search outward from i for the closest decoded frame
```

## 3. Loader: the house builds, then becomes a window

```
geometry (homeMark.js, 200 × 260 units):
  walls x 60 → 140, eaves y 122, apex y 64, base y 220
  outline = left wall → roof apex → chimney → right roof → right wall
  floors  = horizontal lines every 9 units from the base up to the eaves
  attic   = circle at (100, 100), r 7

intro timeline (paused; its progress is set directly, never played):
  floors scale in, bottom → top, staggered
  ground line grows from the centre
  "AURELIA ZURI" fades up
  outline draws (stroke-dashoffset length → 0)
  attic window pops in

every frame:
  elapsed = seconds since start / 2.6
  target  = min(real progress, elapsed)      # never faster than 2.6 s
  shown  += (target - shown) * 0.09          # eased
  intro.progress(shown); counter = shown * 100
  when shown == 1: wait 0.35 s → play outro

outro:
  floors fade out
  the house shape cut out of the near-black veil scales 0 → 1 (a window)
  onReveal()                                 # hero starts its entrance
  window + art scale ×16 (expo.in) and fade
  onDone()                                   # loader unmounts, scroll unlocks
```

## 4. Experience: the scroll-scrubbed tower film

```
section height = 960vh, inner sticky viewport
state = { frame: 0, fit: 0 }

paint():
  i = nearestLoaded(round(state.frame))
  drawFrame(canvas, images[i], fit = state.fit, span = tour.fitSpan)
     # cover crop; as fit → 1 the crop eases to keep the whole tower in view

timeline scrubbed by scroll (0 → 1 over the section):
  0.00 → 0.80   frame 0 → 299             (the film)
  0.80 → 0.88   frame 299 → 233, fit 0 → 1 (camera rises back up the facade)
  ≥ 0.91        live = true               (floor chooser switches on)

  text beats (positions scaled to the film's share of the scroll):
    hero leaves           0.00 → 0.10     blur + fade, hero gradient fades
    "From the ground up"  0.30 → 0.53     dark shade in, lines slide up, out
    "Two worlds, one address" 0.60 → 0.99 neutral dark gradient in, lines + body in, out

on revealed:
  canvas scale 1.14 → 1, hero lines slide up, tagline fades in
```

## 5. Floor chooser and interior walkthrough

```
FloorTour(live):
  mode  = tower | moving | inside
  level = 4, room = rooms[0]

  tower mode (live):
    floor bands are positioned over the tower using source-pixel top/bottom,
    mapped through the same crop the canvas used
    hover / focus → highlight band, show card (level, type, area, note)
    click (touch: first tap selects, second enters) → enter(level)

  enter(level):
    mode = moving; reset every room to bare
    if a fly-in clip exists: play its frames on the clip canvas
    else: zoom toward the balcony glass x, pulling it to centre,
          brighten → warm flash → show the inside layer
    mode = inside

  inside, scroll input (wheel, swipe, arrow keys):
    collect input; a pause > 260 ms or a direction change resets it
    when collected ≥ 0.3 stage and not locked:
      step ±1; lock for 750 ms
      forward past the last stage  → next room (or leave() after the last room)
      backward past the bare shell → previous room
    Esc → leave()

  RoomBuild(room, progress p in 0 → stages):
    base layer   = bare photo
    each stage k = finished photo masked by soft SVG polygons (blurred edges)
    opacity_k    = smoothstep(clamp(p - (k - 1)))
    focus glides toward the stage's focus point on tall screens

  goRoom(id):    push through the hotspot, land in the next room,
                 restore how far that room was furnished
  leave():       pull back out to the tower, bloom the brand mark, mode = tower
  page scrolled away while inside → hard reset to tower
```

## 6. Chapters: shared behaviour

```
Chapters:
  page-wide, once per element:
    [data-lines=auto]  masked lines slide up when the title enters (88% down)
    [data-reveal]      fade up at 92%
    [data-speed=s]     parallax y from -s·0.5vh to +s·0.5vh across the viewport

  every frame:
    hit = chapter whose box covers the nav line (48 px from the top)
    <html data-tone> = hit.tone           # nav switches between light and dark
    rail number = hit.num; rail fill = scroll progress inside hit

Title(lines):  each line in a mask; a line starting with "_" is italic
               (orange on cream pages)
```

## 7. Chapter recipes

```
Everyday:     four arch cards rise on scroll; click → FLIP the arch rect to full screen
Amenities:    pinned list; scroll progress → active index → highlight + photo wipe
Wellness:     pin, translate the track by -(scrollWidth - viewport);
              children with data-drift move at their own rate;
              gym video plays only while near the screen
Water:        pinned; video scales 1.2 → 1; headline in;
              cream panel rises with an arched top that flattens; nav tone flips at 62%
Sky terrace:  card clip-path inset(18% 22% 0 22%) → inset(0); video scale 1.3 → 1
Generations:  arch clip reveal; ghost word "together" x +12% → -28% across the lower row;
              lower row height = max(64vh, 16vh + 26vw), just under the ghost word
Home:         centre interior grows; side rooms drift at different speeds
Considered:   left column sticky; each fact on the right toggles active at 55%;
              the active fact's photo crossfades in the left frame
Location:     pinned aerial; headline gives way to drive times that count up
Wordmark:     giant ZURI with an arch window onto the finished tower;
              "Choose your floor" scrolls back to the floor chooser
Contact:      enquiry form (front end only; connect to a CRM or email service)
Developer:    the loader's house builds again (floors → ground → outline → attic),
              then "AURELIA DEVELOPMENTS" rises letter by letter

Video helper (Water, Sky terrace, Wellness):
  <video muted loop playsInline preload="none" poster=photo>
  IntersectionObserver(rootMargin 25%): intersecting → play(), else pause()
```

## 8. Smooth scroll

```
useLenis(enabled):
  lenis = new Lenis(duration 1.6, exponential ease; 0 when reduced motion)
  lenis.on(scroll) → ScrollTrigger.update
  gsap.ticker drives lenis.raf(time)       # one clock for both
  enabled ? lenis.start(), unlock html : lenis.stop(), lock html, scroll to top
```
