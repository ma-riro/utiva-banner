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

Both mobile and desktop use the same pattern:

1. Add `.fading-out` → word slides up and fades out (`translateY(-100%)`)
2. Swap `textContent` with transition disabled
3. Add `.fading-in` → word is below the slot, invisible
4. Double `requestAnimationFrame` → remove `.fading-in` → word slides up into place

Each word slot has a **`overflow: hidden` wrapper** so the word clips cleanly within its line and never bleeds into the text above.

Desktop rotates on a 3s interval. On desktop the word also changes when hovering a different panel (`PANEL_WORDS = { uti, bladder, menopause }`).

### Key easing

All desktop transitions use `cubic-bezier(0.76, 0, 0.24, 1)` — a strong ease-in-out that feels deliberate and premium.
