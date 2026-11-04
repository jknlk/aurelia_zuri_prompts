# Website prompt: Aurelia Zuri

Use this prompt to brief a designer, a developer or an AI coding agent to build the Aurelia Zuri website from scratch. It describes the site as it is built in [jknlk/aurelia_zuri](https://github.com/jknlk/aurelia_zuri).

---

## The prompt

> Build a cinematic, scroll-driven marketing website for **Aurelia Zuri**, a seven-floor residential tower in Dubai by **Aurelia Developments**. The idea behind the brand: *rooted in Africa, rising in Dubai*. "Zuri" is Swahili for beautiful, a name carried from Africa to a home built in Dubai, for people who live between two continents.
>
> The site should feel like a luxury property film: quiet, slow, editorial, with large serif headlines and a lot of space. It is one long page. Scrolling drives everything; nothing autoplays fast.
>
> **Brand palette (use only these three colours, plus their tints):**
> - Chocolate Brown `#4E342E`: dark backgrounds, text on cream
> - Cream `#FFF8E7`: light pages, text on brown
> - Burnt Orange `#CC5500`: accent only (Enquire pill, italic headline words and labels on cream pages, rules, scroll rail, map pins, buttons). Never use orange for text on brown, because the contrast is too low.
>
> **Type:** Instrument Serif (headlines, with an italic second line) and Inter (body, letter-spaced uppercase labels).
>
> **1. Loader.** A near-black screen (`#0b0b0a`). A line drawing of a house builds itself as the frames load: horizontal orange "floors" stack upward, a cream ground line grows, the cream outline (walls, gable roof, chimney) draws in one stroke, and a round attic window appears. "AURELIA ZURI" fades in below. The loader's progress follows the real image loading but always plays for at least 2.6 s. When it finishes, the house becomes a window: it scales up until it swallows the screen and reveals the film behind it.
>
> **2. The tower film.** A 300-frame image sequence drawn on a full-screen canvas and scrubbed by scroll (about 9.6 screen heights long, pinned). It tells the story of an empty plot, construction, and the finished tower. Text beats appear over it:
> - Hero: "Aurelia / Zuri" with "Rooted in Africa · Rising in Dubai". It blurs away as the clouds part.
> - "From the ground up." with "Seven residential floors · One landmark address", over a darkened empty plot.
> - "Two worlds, one address." with the Zuri story, as construction begins. Use a neutral dark gradient behind it, not a coloured one.
> At the end of the film the camera rises back up the facade until the whole tower is in view.
>
> **3. Choose your floor.** The finished-tower frame becomes interactive. Hovering lights up one floor band at a time and shows a card (level, unit type, area, a short note). Clicking a floor flies the camera in through the balcony glass (zoom, warm flash) into an interior walkthrough. Inside, each room starts as a bare shell and furnishes itself stage by stage as you scroll: furniture fades in region by region (soft-edged polygons over a matching "finished" photo from the same camera). Keep scrolling and you move to the next room (Living, Dining, Main Bedroom), and after the last room you pull back out to the tower. Hotspots link between rooms once a room is complete. Esc leaves the apartment.
>
> **4. Chapters after the film.** Each has a number in a left scroll rail (01, 02, …) and the nav text switches between light and dark to match the section under it.
> 1. **Step inside the everyday.** Four arches (Living Room, Kitchen, Bedroom, Bathroom) rise from the bottom as you scroll. Clicking one opens that room full screen, with the arch growing to fill it.
> 2. **Twelve ways to spend a day.** A pinned amenities list: scrolling moves the highlight down the list while the photo beside it wipes to the matching image.
> 3. **Wellness & wellbeing.** The page pins and travels sideways through four panels ("Breathe in a little Africa.", "Stillness, by design.", "Strength in motion." with a gym video, "Return to yourself."). Headlines and photos drift at different speeds.
> 4. **The water.** A full-screen looping pool video: "Nothing to do. Everything to feel." A cream panel with an arched top rises over it: "Less rush. More Zuri."
> 5. **The sky terrace.** A rooftop video rises from the cream page as a card and opens out to full screen: "Closer to the sky."
> 6. **For every generation.** An arch photo of the kids' garden beside the headline, then a second row under a giant faint ghost word ("together") that slides across as you scroll. No dead space after it.
> 7. **The luxury of coming home.** A brown chapter: a large interior opens up in the centre while smaller rooms drift past.
> 8. **Life, considered.** The headline and a photo hold on the left while key facts scroll past on the right (ceiling height 3.65 m, Italian kitchens, intuitive smart home, open layouts). The photo follows the fact in the middle of the screen.
> 9. **Your world. Within reach.** A toned aerial photo; drive times count up as they arrive (Dubai Marina 5 min, Palm Jumeirah 12, Downtown 18, DXB 25).
> 10. **ZURI wordmark.** A giant "ZURI" with an arch window onto the finished tower: "Make room for a different rhythm." and "Explore Residences / Choose your floor", which scrolls back up to the floor chooser.
> 11. **Contact.** "Let's talk about Aurelia Zuri." with an enquiry form (name, email, phone, message) and contact details.
> 12. **Developer sign-off.** The house drawing from the loader builds itself again above "AURELIA DEVELOPMENTS", which rises letter by letter, followed by a short line about the developer and a footer.
>
> **Navigation.** A fixed top bar: "AURELIA DEVELOPMENTS" on the left; Location, Residences and an always-visible orange **Enquire** pill on the right; a burger that opens a full-screen menu (The Story, The Tower, Step Inside, Interiors, Amenities, Wellness, Location, Residences, The Developer, Contact), each with a short note. Menu items scroll smoothly either to a point inside the film or to a section.
>
> **Motion rules.**
> - Smooth scrolling throughout.
> - Headlines arrive as masked lines that slide up.
> - Photos have gentle parallax.
> - Videos are muted, looping and inline, and only load and play while near the screen.
> - Respect `prefers-reduced-motion`: smooth scrolling off, moves collapse to cuts.
> - Works on phones: 16 px gutters, stacked layouts, touch and swipe in the tour.
>
> **Content lives in config files**, not in components, so copy, images, floor data and timing can change without touching layout code.

---

## Follow-up prompts used while building

These are the revision requests that shaped the current site, in order:

1. "Use the brand palette on every page: Chocolate Brown #4E342E, Cream #FFF8E7, Burnt Orange #CC5500."
2. "Put the gym video in the wellness section." (`public/vdos/wellness&wellbeing`)
3. "Access this video and have it on the water section." (`public/vdos/water`)
4. "Remove the masterplan." (The aerial site plan with pins was taken off the page.)
5. "Place the video in the sky terrace section." (`public/vdos/skyterrace`)
6. "Don't have this much long gap." (Trimmed the empty space under the "together" ghost word.)
7. "Use the design from the first page." (The footer arch became the loader's house drawing.)
8. "Use the first background colour that was there before, only for this." (The loader keeps its original near-black background.)
9. "Don't have an orange fade here." (The gradient behind "Two worlds, one address" went back to neutral black.)
