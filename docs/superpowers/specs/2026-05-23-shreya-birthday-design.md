# Shreya's Birthday Website — Design Spec
**Date:** 2026-05-23  
**Occasion:** Shreya's 20th Birthday — June 14th  
**Vibe:** Romantic & Dreamy  
**Stack:** Single `index.html` · Three.js (CDN) · GSAP + ScrollTrigger (CDN) · No build tools

---

## Overview

A two-state birthday website. Before June 14th 12:00 AM, Shreya sees only a locked countdown screen. At midnight, the page automatically unlocks with a cinematic reveal and the full romantic experience becomes visible.

---

## State 1 — Locked (Before June 14th 12:00 AM)

**Goal:** Create suspense and anticipation. Nothing about the content inside is revealed.

### Visual Design
- Full-screen dark background: deep midnight `#0a0a14` → `#1a0a2e` gradient
- Subtle animated star field (pure CSS, no Three.js needed here — keep it lightweight)
- Centered layout with vertical stack

### Elements
1. **Lock icon** — large `🔒` with a soft glowing pulse animation
2. **Her name** — `Shreya` in italic Dancing Script font, rose-pink `#f8a5c2`, large (~3.5rem)
3. **Tagline** — `"Something beautiful is waiting for you…"` in small uppercase, muted color
4. **Live countdown** — four glass-morphism tiles (Days : Hours : Minutes : Seconds), updating every second via `setInterval`
5. **Hint** — `"This page will unlock on June 14th at midnight ✨"` in small italic, very muted

### Behavior
- `setInterval` runs every 1000ms, recalculating time to `2026-06-14T00:00:00` in local time
- If countdown reaches zero → trigger **unlock sequence** (see below)
- All unlocked sections exist in the DOM at load time inside `<div id="main-content" style="display:none">` — they are revealed (not injected) on unlock, but hidden content requires Dev Tools to find since there's no visible hint

---

## The Unlock Sequence (Midnight Reveal)

This is the cinematic moment. Should feel like a gift being unwrapped.

### Animation Timeline (≈4 seconds)
1. **0.0s** — Countdown hits `00:00:00:00`, tiles freeze
2. **0.3s** — Screen flashes white briefly (like a camera flash)
3. **0.5s** — Lock icon animates: shakes, then breaks apart (CSS keyframe)
4. **0.8s** — Locked screen fades out + slides upward out of view
5. **1.2s** — Three.js canvas fades in behind — 3D rose petals & hearts burst outward from center like a firework explosion
6. **2.0s** — "Happy Birthday Shreya!" text animates in character by character (GSAP SplitText-style, manual split)
7. **2.8s** — Confetti rains down (CSS animation, ~50 colored divs)
8. **3.5s** — Subtitle and scroll-down arrow fade in
9. **4.0s** — Background music (Tum Hi Ho) begins playing (YouTube iframe unmutes)
10. **4.5s** — Full page sections become scrollable

---

## State 2 — Unlocked (Full Birthday Experience)

### Architecture
All sections live in a `<div id="main-content">` that has `display:none` initially. On unlock, it's revealed. Three.js canvas runs as a fixed background layer (z-index: -1) throughout.

### Section 1 — Hero
- Fixed Three.js particle background: ~300 rose petals + hearts floating in 3D space, rotating slowly, with mouse-parallax (petals subtly follow cursor)
- Centered text: `Happy Birthday,` (small, uppercase, tracked) + `Shreya` (massive, gradient text, Dancing Script) + `You are 20 ✨` badge
- Subtle scroll-down arrow bouncing at bottom

### Section 2 — Love Letter
- Full-width section, dark background with soft vignette
- `"A Letter For You"` label in small caps, rose gold
- Letter text (3–4 paragraphs, written by Claude, romantic) in Cormorant Garamond italic, cream color
- GSAP ScrollTrigger: text lines fade in one by one as she scrolls — typewriter-reveal effect
- Decorative: thin horizontal rose-gold lines top and bottom, small `💌` icon

### Section 3 — 20 Reasons I Love You
- Section title: `"20 Reasons I Love You"` (one for each year)
- 20 cards in a responsive grid (4 columns desktop, 2 mobile)
- Each card: number (01–20) in rose gold, reason text below, subtle pink glow border
- GSAP ScrollTrigger: cards cascade in from bottom as she scrolls, staggered 80ms apart
- Card hover: soft lift + glow intensifies

### Section 4 — Photo Gallery
- Section title: `"Us 💕"`
- 6 polaroid-style frames arranged in a slightly rotated scattered layout
- Each polaroid: white frame, photo placeholder (gradient pink), caption slot at bottom (empty, for user to add)
- Hover: card tilts toward cursor (CSS perspective transform)
- Placeholder images: soft pink/rose gradients with `📸` icon and `[Your photo here]` caption

### Section 5 — Music Player (Tum Hi Ho)
- Small sticky floating music player bottom-right corner (visible throughout)
- Shows: 🎵 icon + "Tum Hi Ho · Arijit Singh" + animated equalizer bars
- Hidden YouTube iframe (`width:0; height:0`) autoloads on unlock
- Play/pause toggle on click
- On mobile: player is fixed bottom-center

---

## Typography
| Role | Font | Weight |
|------|------|--------|
| Display names | Dancing Script (Google) | 700 |
| Body / letter | Cormorant Garamond (Google) | 400 italic |
| Labels / UI | Montserrat (Google) | 300–400 |

---

## Color Palette
| Name | Hex | Use |
|------|-----|-----|
| Deep Night | `#0a0a14` | Page background |
| Midnight Purple | `#1a0020` | Section backgrounds |
| Rose Pink | `#ff6b9d` | Primary accent, headings |
| Soft Pink | `#f8a5c2` | Secondary accent, subtitles |
| Blush | `#ffd3e8` | Text on dark backgrounds |
| Rose Gold | `#c9965a` | Decorative lines, numbers |

---

## Three.js Particle System
- ~300 particles: mix of small hearts (`♥`) and petal shapes (custom geometry or sprite textures)
- Rendered on a full-screen canvas, fixed position, z-index: -1
- Particles drift upward/sideways slowly with slight sine wave oscillation
- On unlock: burst animation — all particles shoot outward from center, then settle into drift
- Mouse parallax: particles shift slightly opposite to cursor direction (subtle depth)
- Performance: capped at 60fps with `requestAnimationFrame`, reduced particle count on mobile

---

## Data Flow
```
Browser loads index.html
  → setInterval checks: Date.now() < June 14 12:00 AM?
      YES → show countdown, update every second
      NO  → skip directly to unlock sequence
  → Unlock sequence runs (GSAP timeline)
  → Three.js scene initializes
  → Sections injected/revealed
  → Music player activates
```

---

## File Structure
```
shreya-birthday/
  index.html        ← entire site (single file)
  photos/           ← placeholder, user adds real photos here
    photo-1.jpg ... photo-6.jpg
  README.md         ← instructions for personalizing (letter, reasons, photos)
```

---

## Content (Written by Claude, to be personalized)

### Love Letter (sample — user will edit)
> "Shreya, 20 years ago the world quietly became more beautiful without anyone quite noticing — except me.  
> Every moment I've spent with you has felt like the first chapter of a story I never want to end. Your laugh, your kindness, the way you see the world — these are the things I carry with me everywhere.  
> On this day, I want you to know that you are loved beyond every word I know how to say. Happy birthday, jaan.  
> — Always yours 💕"

### 20 Reasons (sample — user will edit)
01. Your smile that makes everything okay  
02. The way you laugh at your own jokes  
03. How kind you are to everyone around you  
04. Your strength — even when things are hard  
05. The way you make ordinary moments feel special  
06. Your eyes that say more than words  
07. How you always know what to say  
08. Your passion for everything you love  
09. The way you make me feel seen  
10. Your honesty, even when it's hard  
11. How you love with your whole heart  
12. Your voice — talking, singing, everything  
13. The little things you do without thinking  
14. How you make every place feel like home  
15. Your courage to be exactly yourself  
16. The way you care for people around you  
17. Your dreams — and how big they are  
18. How you've changed my world for the better  
19. Every memory we've made together  
20. Simply that you exist — and you're mine 💕

---

## Music
- Song: **Tum Hi Ho** — Arijit Singh (Aashiqui 2)
- YouTube Video ID: `Umqb9KENgmk`
- Embed URL: `https://www.youtube.com/embed/Umqb9KENgmk?autoplay=1&mute=1&loop=1&playlist=Umqb9KENgmk`
- Embed: YouTube iframe, `autoplay=1&mute=1`, unmuted via JS on unlock
- Fallback: if YouTube blocked, player shows "Click to play" with direct link

---

## Responsiveness
- Mobile-first layout; all sections stack vertically on phones
- Three.js particle count reduced to 150 on `window.innerWidth < 768`
- Polaroid gallery scrolls horizontally on mobile
- Countdown tiles shrink gracefully on small screens

---

## No Dependencies to Install
Everything via CDN:
- Three.js r128: `https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js`
- GSAP 3 + ScrollTrigger: `https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js`
- Google Fonts: loaded via `<link>` in `<head>`
