# Utiva Banner

Responsive hero banner for Utiva — a women's health brand. Built as a single HTML file with no dependencies.

**Live demo:** https://utiva-banner.vercel.app

---

## Features

- **Desktop (≥768px):** Three overlapping panels (UTI Prevention, Bladder Control, Menopause Relief) that slide and reveal on hover, with smooth panel transitions, photo scale, and button animations
- **Mobile:** Scroll-driven frame stack — three full-screen frames that stack and reveal as the user scrolls
- **Rotating headline:** "Take control of your *confidence / comfort / peace of mind*" — word swaps with a clean clip animation on both mobile and desktop
- **Responsive:** Uses CSS container queries (`cqw`) capped at 1440px so proportions stay pixel-perfect at any viewport width

## Stack

- Vanilla HTML / CSS / JS — no frameworks, no build step
- [General Sans](https://www.fontshare.com/fonts/general-sans) via Fontshare
- Hosted on Vercel

## Structure

```
utiva-banner/
├── index.html        # All HTML, CSS, and JS in one file
├── assets/
│   ├── uti.jpg       # UTI panel photo
│   ├── bladder.jpg   # Bladder panel photo
│   ├── meno.jpg      # Menopause panel photo
│   ├── badge.png     # #1 Doctor Recommended badge
│   ├── avatar1.jpg   # Social proof avatars
│   ├── avatar2.jpg
│   ├── avatar3.jpg
│   └── avatar4.jpg
└── vercel.json       # Vercel static site config
```

## Local development

No install needed — just serve the folder:

```bash
python3 -m http.server 3456
# open http://localhost:3456
```

## Deployment

```bash
vercel --prod
```

---

## Animation system

### Mobile — scroll-driven frames

Three frames (`#frame1` UTI, `#frame2` Bladder, `#frame3` Menopause) are stacked absolutely inside a `.sticky-stage`. The scroll wrapper is `300vh` tall, so the stage stays pinned while the user scrolls through three "pages".

Two progress values drive everything:

```js
const p1 = scrollY / vh          // 0→1 as frame 1 scrolls away
const p2 = (scrollY - vh) / vh   // 0→1 as frame 2 scrolls away
```

| What                  | How                                              |
|-----------------------|--------------------------------------------------|
| Frame slides up       | `frame1.style.transform = translateY(-p1 * 100%)` |
| Dark overlay fades    | `overlay2.opacity = 1 - p1` (fully dark when covered, clear when revealed) |
| Frame peeks from bottom | `frame2.bottom` grows from 0→48px at p1 > 0.65 |
| CTA crossfade         | Overlapping opacity curves so buttons fade in before the previous one is gone — no dead zone |

### Desktop — hover state machine

A single class on `.desktop-hero` drives everything:

```
.state-uti  →  .state-bladder  →  .state-menopause
```

Switching state class triggers CSS transitions on:
- **Panel position** (`left`) — panels slide left/right (`0.7s cubic-bezier(0.76,0,0.24,1)`)
- **Overlay opacity** — inactive panels darken (`0.7s` with `0.08s` delay so slide leads)
- **Photo scale** — active panel image scales to `1.04` (`0.9s`)
- **Button fill** — active button turns red with a shadow
- **Button icon** — arrow icon expands from `width: 0` to full size (no `display:none` snap)

Hover zones (`.dh-zone`) are invisible divs that sit above the panels and call `activatePanel(state)`.

### Word rotation

Both mobile and desktop use the same pattern with an **overlapping crossfade** so the transition feels continuous:

1. Add `.fading-out` → word slides up 65% and blurs out (`translateY(-65%) + filter:blur(6px)`)
2. At **300ms** (55% through the 550ms exit) — swap `textContent` while transition is disabled
3. Add `.fading-in` → word starts below the slot, blurred and invisible
4. Double `requestAnimationFrame` → remove `.fading-in` → word rises into place, sharpening as it arrives

The key detail: swapping at 300ms means the incoming word starts entering **before** the outgoing word has fully left. This overlap is what makes it feel fluid rather than two separate animations.

Each word slot sits inside an **`overflow: hidden` wrapper** so words clip cleanly within their line — no bleeding into the text above.

| Property | Mobile | Desktop |
|----------|--------|---------|
| Duration | 550ms | 600ms |
| Travel | 65% | 65% |
| Blur | 6px | 8px |
| Swap at | 300ms | 330ms |
| Interval | 3.5s | on hover |

### Key easing

All desktop transitions use `cubic-bezier(0.76, 0, 0.24, 1)` — a strong ease-in-out that feels deliberate and premium.
