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
