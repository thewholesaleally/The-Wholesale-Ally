# The Wholesale Ally

A one-page site for The Wholesale Ally — land wholesaling courses, contracts,
builder resources, and community access, with real Stripe checkout wired
straight into the page. Vanilla HTML, CSS, and JavaScript. No framework, no
build step, no dependencies.

**Live structure:** hero → mission → course carousel → Buy Box Bundle →
a-la-carte pricing → demo gallery → footer, plus a persistent music player
and a small animated mascot in the corners.

## Features

- **Course carousel** — Wholesale Ally 101 (free), Let's Change Our Life,
  Now We're Making Money, Ground Runnin', and the Buy Box Bundle, each
  opening a popup with its promo video (or an animated cover mockup as a
  fallback if no video is present yet) and a "what's included" list.
- **"Pick What You Need" pricing grid** — six a la carte items (Assignment
  Contract, Addendum, Purchase & Sale, Builders List, KindSkip Leads,
  Discord access), each its own Stripe Payment Link.
- **Demo gallery** — hover-to-flip cards (LiveMock Simulator, Zillow
  Scanner, Contract Read-Through) showing a short pitch on the front and
  four benefit bullets on the back; clicking one opens its walkthrough
  video.
- **iPod-styled music player** — bottom-right, click-wheel UI, attempts
  autoplay on the visitor's first click/tap (browsers block true autoplay
  until then — that's a browser rule, not something the page controls).
- **Bear mascot** — bottom-left, hand-drawn in SVG (no image assets),
  sleeps continuously with a floating dream-word bubble above it.
- **All checkout links, pricing, and media paths live in one config block**
  at the top of `app.js` — nothing is hardcoded into the HTML.

## File structure

```
index.html          All page markup
styles.css           Design system: colors, type, every component's styling
app.js               Config (links, prices, playlist) + all interactivity
assets/
  fonts/              Fraunces (display) + Jost (body/UI)
  img/                Logo, course covers, a-la-carte product thumbnails
  video/               Course promo videos, plus video/demos/ for the
                        LiveMock / Zillow Scanner / CRT walkthroughs
  audio/               Background tracks for the iPod player
```

## Running it locally

No build step — open `index.html` directly in a browser, or serve the
folder with anything static:

```bash
python3 -m http.server 8000
```

Opening `index.html` by double-clicking a file **inside an unopened zip**
(without extracting first) will break relative paths for the CSS, JS, and
every asset — extract the zip to a real folder first.

## Deploying

Any static host works — drag the folder onto
[Netlify Drop](https://app.netlify.com/drop) for a live URL in seconds, or
push this repo to GitHub Pages, Vercel, or your own hosting.

## Configuration — before going live

All of this lives at the top of `app.js`:

### 1. Stripe Payment Links
`STRIPE_LINKS` (courses + bundle) and `ALACARTE_LINKS` (the six standalone
items) each need a real
[Stripe Payment Link](https://dashboard.stripe.com/payment-links).
`WA101_FREE_LINK` should point wherever the free starter PDF is hosted for
direct download.

### 2. Music
Drop mp3/m4a files into `assets/audio/` and list them in the `PLAYLIST`
array:

```js
const PLAYLIST = [
  { title: "Track Name — Artist", src: "assets/audio/track-file.mp3" },
];
```

### 3. Course & demo videos
`COURSE_VIDEO` and each entry in `DEMOS` point to an mp4 path. If a file
isn't present at that path yet, course cards fall back to an animated
cover mockup automatically — the site stays presentable either way.

## Design system

Colors and type are defined once, at the top of `styles.css`, as CSS
variables — change one line to update the whole site:

| Token | Value | Use |
|---|---|---|
| `--forest` | `#0B2A1E` | Primary dark background |
| `--cream` | `#F6F3E6` | Page background |
| `--gold` | `#B8903F` | Accent / CTAs |
| `--clay` | `#A85630` | Secondary accent |

Fonts: **Fraunces** (serif, headings/display) and **Jost** (sans, body/UI),
both self-hosted in `assets/fonts/`.

## Browser support notes

- CSS Grid and 3D transforms (`backface-visibility`, `rotateY`) are used
  for the pricing grid and demo flip cards — standard in every modern
  browser for years, no polyfills needed.
- Audio autoplay is intentionally gated behind a user gesture (click or
  tap), per browser policy — see the Music section above.
