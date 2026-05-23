# Shreya Birthday Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-file romantic birthday website for Shreya with a locked countdown state that dramatically unlocks at June 14th midnight with 3D particles, GSAP animations, and a full scrollable experience.

**Architecture:** Single `index.html` containing all HTML, CSS, and JS. Locked state shows only a countdown; at midnight a cinematic GSAP timeline plays, then Three.js particles + all sections reveal. All content is in the DOM at load, hidden via `display:none`.

**Tech Stack:** Three.js r128 (CDN), GSAP 3 + ScrollTrigger (CDN), Google Fonts (Dancing Script, Cormorant Garamond, Montserrat), YouTube iframe for music, vanilla JS — no build step.

---

## File Map

| File | Purpose |
|------|---------|
| `shreya-birthday/index.html` | Entire site — all HTML, CSS, JS inline |
| `shreya-birthday/photos/photo-1.jpg … photo-6.jpg` | Placeholder images (not created — user adds real photos) |
| `shreya-birthday/README.md` | Instructions for personalizing content & swapping photos |

---

### Task 1: HTML skeleton + locked countdown screen

**Files:**
- Create: `shreya-birthday/index.html`

- [ ] **Step 1: Create base HTML with fonts, CDN scripts, and locked-state markup**

Create `shreya-birthday/index.html` with this exact content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Happy Birthday Shreya 💕</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Cormorant+Garamond:ital,wght@1,400;1,600&family=Montserrat:wght@300;400;600&display=swap" rel="stylesheet" />
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>
  <style>
    /* ── Reset & base ── */
    *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }
    :root {
      --deep-night: #0a0a14;
      --midnight:   #1a0020;
      --rose-pink:  #ff6b9d;
      --soft-pink:  #f8a5c2;
      --blush:      #ffd3e8;
      --rose-gold:  #c9965a;
    }
    html { scroll-behavior: smooth; }
    body {
      background: var(--deep-night);
      color: var(--blush);
      font-family: 'Montserrat', sans-serif;
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* ── Locked screen ── */
    #locked-screen {
      position: fixed; inset: 0;
      display: flex; flex-direction: column;
      align-items: center; justify-content: center;
      gap: 20px;
      background: radial-gradient(ellipse at center, #1a0a2e 0%, #0a0a14 100%);
      z-index: 1000;
    }
    /* CSS star field */
    #locked-screen::before {
      content: '';
      position: absolute; inset: 0;
      background-image:
        radial-gradient(1px 1px at 10% 15%, rgba(255,255,255,0.4) 0%, transparent 100%),
        radial-gradient(1px 1px at 25% 35%, rgba(255,255,255,0.3) 0%, transparent 100%),
        radial-gradient(2px 2px at 40% 10%, rgba(255,255,255,0.35) 0%, transparent 100%),
        radial-gradient(1px 1px at 55% 60%, rgba(255,255,255,0.25) 0%, transparent 100%),
        radial-gradient(1px 1px at 70% 25%, rgba(255,255,255,0.4) 0%, transparent 100%),
        radial-gradient(2px 2px at 80% 70%, rgba(255,255,255,0.3) 0%, transparent 100%),
        radial-gradient(1px 1px at 90% 45%, rgba(255,255,255,0.35) 0%, transparent 100%),
        radial-gradient(1px 1px at 15% 75%, rgba(255,255,255,0.25) 0%, transparent 100%),
        radial-gradient(1px 1px at 60% 85%, rgba(255,255,255,0.3) 0%, transparent 100%),
        radial-gradient(2px 2px at 35% 55%, rgba(255,255,255,0.2) 0%, transparent 100%);
      pointer-events: none;
    }
    .lock-icon {
      font-size: 3.5rem;
      animation: pulse-glow 2.5s ease-in-out infinite;
    }
    @keyframes pulse-glow {
      0%, 100% { filter: drop-shadow(0 0 8px rgba(255,107,157,0.4)); transform: scale(1); }
      50%       { filter: drop-shadow(0 0 20px rgba(255,107,157,0.8)); transform: scale(1.08); }
    }
    .locked-name {
      font-family: 'Dancing Script', cursive;
      font-size: clamp(2.5rem, 8vw, 4rem);
      color: var(--soft-pink);
      letter-spacing: 2px;
    }
    .locked-tagline {
      font-size: clamp(0.65rem, 2vw, 0.8rem);
      letter-spacing: 4px;
      text-transform: uppercase;
      color: rgba(255,255,255,0.35);
      text-align: center;
    }
    /* Countdown tiles */
    .countdown-row {
      display: flex; align-items: center; gap: 10px;
      margin-top: 8px;
    }
    .cd-unit { text-align: center; }
    .cd-num {
      display: block;
      font-size: clamp(1.8rem, 6vw, 2.6rem);
      font-weight: 600;
      color: #fff;
      background: rgba(255,255,255,0.05);
      border: 1px solid rgba(255,255,255,0.12);
      border-radius: 12px;
      padding: 10px 16px;
      min-width: clamp(54px, 12vw, 72px);
      backdrop-filter: blur(4px);
      font-variant-numeric: tabular-nums;
    }
    .cd-lbl {
      display: block;
      font-size: 0.55rem;
      color: rgba(255,255,255,0.3);
      letter-spacing: 3px;
      text-transform: uppercase;
      margin-top: 6px;
    }
    .cd-sep {
      font-size: 2rem;
      color: rgba(255,107,157,0.4);
      margin-bottom: 18px;
      font-weight: 300;
    }
    .locked-hint {
      font-size: 0.72rem;
      color: rgba(255,255,255,0.2);
      font-style: italic;
      text-align: center;
      margin-top: 4px;
    }

    /* ── Flash overlay ── */
    #flash {
      position: fixed; inset: 0;
      background: #fff;
      opacity: 0; pointer-events: none;
      z-index: 2000;
    }

    /* ── Three.js canvas ── */
    #bg-canvas {
      position: fixed; inset: 0;
      z-index: -1;
      opacity: 0;
    }

    /* ── Main content (hidden until unlock) ── */
    #main-content { display: none; }

  </style>
</head>
<body>

<!-- Flash overlay for cinematic reveal -->
<div id="flash"></div>

<!-- Locked countdown screen -->
<div id="locked-screen">
  <div class="lock-icon">🔒</div>
  <div class="locked-name">Shreya</div>
  <div class="locked-tagline">Something beautiful is waiting for you…</div>
  <div class="countdown-row">
    <div class="cd-unit"><span class="cd-num" id="cd-days">00</span><span class="cd-lbl">Days</span></div>
    <div class="cd-sep">:</div>
    <div class="cd-unit"><span class="cd-num" id="cd-hours">00</span><span class="cd-lbl">Hours</span></div>
    <div class="cd-sep">:</div>
    <div class="cd-unit"><span class="cd-num" id="cd-mins">00</span><span class="cd-lbl">Mins</span></div>
    <div class="cd-sep">:</div>
    <div class="cd-unit"><span class="cd-num" id="cd-secs">00</span><span class="cd-lbl">Secs</span></div>
  </div>
  <div class="locked-hint">This page will unlock on June 14th at midnight ✨</div>
</div>

<!-- Three.js canvas -->
<canvas id="bg-canvas"></canvas>

<!-- All unlocked content — hidden until midnight -->
<div id="main-content">
  <!-- Populated in Task 2 -->
</div>

<script>
  // ── Countdown logic ──
  const UNLOCK_DATE = new Date('2026-06-14T00:00:00');

  function pad(n) { return String(n).padStart(2, '0'); }

  function updateCountdown() {
    const now = new Date();
    const diff = UNLOCK_DATE - now;
    if (diff <= 0) {
      clearInterval(countdownInterval);
      triggerUnlock();
      return;
    }
    const days  = Math.floor(diff / 86400000);
    const hours = Math.floor((diff % 86400000) / 3600000);
    const mins  = Math.floor((diff % 3600000)  / 60000);
    const secs  = Math.floor((diff % 60000)    / 1000);
    document.getElementById('cd-days').textContent  = pad(days);
    document.getElementById('cd-hours').textContent = pad(hours);
    document.getElementById('cd-mins').textContent  = pad(mins);
    document.getElementById('cd-secs').textContent  = pad(secs);
  }

  // Module-level so updateCountdown() can clearInterval on it
  var countdownInterval;

  // Wait for all scripts to load before checking — triggerUnlock() is defined in a later script block
  window.addEventListener('load', function () {
    if (new Date() >= UNLOCK_DATE) {
      triggerUnlock();
    } else {
      updateCountdown();
      countdownInterval = setInterval(updateCountdown, 1000);
    }
  });
</script>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify countdown works**

Open `shreya-birthday/index.html` in a browser. You should see:
- Dark starfield background
- Pulsing 🔒 icon
- "Shreya" in pink cursive
- Live countdown ticking down to June 14 2026

- [ ] **Step 3: Init git and commit**

```bash
cd /Users/dipak/Desktop/claudecode/shreya-birthday
git init
git add index.html
git commit -m "feat: locked countdown screen with live timer"
```

---

### Task 2: Full unlocked content HTML + CSS

**Files:**
- Modify: `shreya-birthday/index.html` — fill `#main-content`, add all section CSS

- [ ] **Step 1: Add section CSS to the `<style>` block** (paste inside `</style>` before it closes)

```css
    /* ══════════════════════════════════════
       SECTIONS — shared
    ══════════════════════════════════════ */
    section {
      position: relative;
      min-height: 100vh;
      display: flex; flex-direction: column;
      align-items: center; justify-content: center;
      padding: 80px 24px;
      overflow: hidden;
    }
    .section-label {
      font-size: 0.65rem;
      letter-spacing: 5px;
      text-transform: uppercase;
      color: var(--rose-gold);
      margin-bottom: 16px;
    }
    .section-title {
      font-family: 'Dancing Script', cursive;
      font-size: clamp(2.2rem, 6vw, 3.5rem);
      color: var(--soft-pink);
      text-align: center;
      margin-bottom: 48px;
    }
    .gold-line {
      width: 80px; height: 1px;
      background: linear-gradient(90deg, transparent, var(--rose-gold), transparent);
      margin: 0 auto 40px;
    }

    /* ══════════════════════════════════════
       HERO SECTION
    ══════════════════════════════════════ */
    #hero {
      min-height: 100vh;
      background: transparent;
      text-align: center;
      gap: 0;
    }
    .hero-top {
      font-size: clamp(0.7rem, 2vw, 0.9rem);
      letter-spacing: 6px;
      text-transform: uppercase;
      color: var(--soft-pink);
      margin-bottom: 16px;
      opacity: 0; /* animated in */
    }
    .hero-name {
      font-family: 'Dancing Script', cursive;
      font-size: clamp(4rem, 15vw, 9rem);
      line-height: 1;
      background: linear-gradient(135deg, #ff6b9d 0%, #f8a5c2 40%, #ffd3e8 70%, #c9965a 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      opacity: 0; /* animated in */
    }
    /* Each character wrapped in a span for letter-by-letter animation */
    .hero-name .char { display: inline-block; }
    .hero-badge {
      display: inline-flex; align-items: center; gap: 8px;
      margin-top: 24px;
      background: rgba(255,107,157,0.12);
      border: 1px solid rgba(255,107,157,0.35);
      border-radius: 30px;
      padding: 8px 22px;
      font-size: 0.8rem;
      letter-spacing: 3px;
      color: var(--blush);
      opacity: 0; /* animated in */
    }
    .scroll-arrow {
      position: absolute; bottom: 36px;
      font-size: 1.4rem;
      animation: bounce-arrow 1.8s ease-in-out infinite;
      color: var(--soft-pink);
      opacity: 0;
    }
    @keyframes bounce-arrow {
      0%, 100% { transform: translateY(0); }
      50%       { transform: translateY(10px); }
    }
    /* Confetti pieces */
    .confetti-piece {
      position: fixed;
      top: -20px;
      width: 8px; height: 8px;
      border-radius: 2px;
      pointer-events: none;
      z-index: 999;
      animation: confetti-fall linear forwards;
    }
    @keyframes confetti-fall {
      to { transform: translateY(110vh) rotate(720deg); opacity: 0; }
    }

    /* ══════════════════════════════════════
       LOVE LETTER SECTION
    ══════════════════════════════════════ */
    #letter {
      background: linear-gradient(180deg, var(--deep-night) 0%, var(--midnight) 50%, var(--deep-night) 100%);
    }
    .letter-wrap {
      max-width: 680px;
      text-align: center;
    }
    .letter-icon {
      font-size: 2.5rem;
      margin-bottom: 20px;
    }
    .letter-body p {
      font-family: 'Cormorant Garamond', serif;
      font-size: clamp(1.05rem, 2.5vw, 1.25rem);
      line-height: 2;
      color: var(--blush);
      margin-bottom: 20px;
      opacity: 0; /* ScrollTrigger reveals each */
      transform: translateY(30px);
    }
    .letter-sign {
      font-family: 'Dancing Script', cursive;
      font-size: 1.6rem;
      color: var(--rose-pink);
      margin-top: 16px;
      opacity: 0;
      transform: translateY(20px);
    }

    /* ══════════════════════════════════════
       20 REASONS SECTION
    ══════════════════════════════════════ */
    #reasons {
      background: var(--deep-night);
    }
    .reasons-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 16px;
      max-width: 960px;
      width: 100%;
    }
    @media (max-width: 900px) { .reasons-grid { grid-template-columns: repeat(3, 1fr); } }
    @media (max-width: 640px) { .reasons-grid { grid-template-columns: repeat(2, 1fr); } }
    .reason-card {
      background: rgba(255,107,157,0.06);
      border: 1px solid rgba(255,107,157,0.18);
      border-radius: 14px;
      padding: 18px 14px;
      text-align: center;
      cursor: default;
      transition: transform 0.3s ease, box-shadow 0.3s ease, border-color 0.3s ease;
      opacity: 0; /* GSAP reveals */
      transform: translateY(40px);
    }
    .reason-card:hover {
      transform: translateY(-6px) scale(1.03);
      box-shadow: 0 12px 40px rgba(255,107,157,0.25);
      border-color: rgba(255,107,157,0.5);
    }
    .reason-num {
      display: block;
      font-size: 1.5rem;
      font-weight: 600;
      color: var(--rose-gold);
      margin-bottom: 8px;
      font-variant-numeric: tabular-nums;
    }
    .reason-text {
      font-family: 'Cormorant Garamond', serif;
      font-size: 0.95rem;
      font-style: italic;
      color: var(--blush);
      line-height: 1.5;
    }

    /* ══════════════════════════════════════
       PHOTO GALLERY SECTION
    ══════════════════════════════════════ */
    #gallery {
      background: linear-gradient(180deg, var(--deep-night), var(--midnight), var(--deep-night));
    }
    .polaroid-wrap {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 24px;
      max-width: 900px;
    }
    .polaroid {
      background: #fff;
      padding: 10px 10px 36px;
      box-shadow: 4px 8px 24px rgba(0,0,0,0.5);
      cursor: pointer;
      transition: transform 0.4s ease, box-shadow 0.4s ease;
      width: 180px;
      transform-style: preserve-3d;
      perspective: 600px;
      opacity: 0; /* GSAP reveals */
    }
    .polaroid:nth-child(odd)  { transform: rotate(-3deg); }
    .polaroid:nth-child(even) { transform: rotate(2.5deg); }
    .polaroid:hover {
      transform: rotate(0deg) scale(1.08) translateY(-8px) !important;
      box-shadow: 8px 20px 48px rgba(0,0,0,0.6);
      z-index: 10;
    }
    .polaroid-img {
      width: 100%; aspect-ratio: 1;
      background: linear-gradient(135deg, #ffb3c6, #ffd3e8, #f8a5c2);
      display: flex; align-items: center; justify-content: center;
      font-size: 2.5rem;
      overflow: hidden;
    }
    .polaroid-img img {
      width: 100%; height: 100%;
      object-fit: cover;
      display: block;
    }
    .polaroid-caption {
      text-align: center;
      margin-top: 8px;
      font-family: 'Dancing Script', cursive;
      font-size: 0.85rem;
      color: #555;
    }

    /* ══════════════════════════════════════
       FOOTER
    ══════════════════════════════════════ */
    #footer {
      min-height: 40vh;
      background: var(--midnight);
      text-align: center;
      padding: 60px 24px;
      font-family: 'Dancing Script', cursive;
      font-size: clamp(1.6rem, 4vw, 2.4rem);
      color: var(--soft-pink);
    }
    #footer p { margin-bottom: 12px; }
    #footer .footer-small {
      font-family: 'Montserrat', sans-serif;
      font-size: 0.7rem;
      letter-spacing: 3px;
      color: rgba(255,255,255,0.2);
      margin-top: 24px;
    }

    /* ══════════════════════════════════════
       FLOATING MUSIC PLAYER
    ══════════════════════════════════════ */
    #music-player {
      position: fixed;
      bottom: 24px; right: 24px;
      background: rgba(10,10,20,0.85);
      border: 1px solid rgba(255,107,157,0.3);
      border-radius: 50px;
      padding: 10px 18px;
      display: flex; align-items: center; gap: 10px;
      cursor: pointer;
      z-index: 500;
      backdrop-filter: blur(12px);
      transition: border-color 0.3s, box-shadow 0.3s;
      user-select: none;
      opacity: 0; /* fades in after unlock */
    }
    #music-player:hover {
      border-color: rgba(255,107,157,0.6);
      box-shadow: 0 4px 20px rgba(255,107,157,0.2);
    }
    .music-icon-el { font-size: 1.1rem; }
    .music-info-wrap { display: flex; flex-direction: column; gap: 3px; }
    .music-title-el { font-size: 0.7rem; color: var(--blush); white-space: nowrap; }
    .music-bars {
      display: flex; gap: 2px; align-items: flex-end; height: 14px;
    }
    .music-bar {
      width: 3px; background: var(--rose-pink); border-radius: 2px;
      animation: eq-bounce 0.7s ease-in-out infinite alternate;
    }
    .music-bar:nth-child(1) { height: 6px;  animation-delay: 0s;    }
    .music-bar:nth-child(2) { height: 12px; animation-delay: 0.15s; }
    .music-bar:nth-child(3) { height: 9px;  animation-delay: 0.3s;  }
    .music-bar:nth-child(4) { height: 14px; animation-delay: 0.1s;  }
    .music-bar:nth-child(5) { height: 7px;  animation-delay: 0.25s; }
    @keyframes eq-bounce {
      from { transform: scaleY(0.3); }
      to   { transform: scaleY(1);   }
    }
    .music-bar.paused { animation-play-state: paused; }

    /* Hidden YouTube iframe */
    #yt-frame { position: fixed; width: 0; height: 0; opacity: 0; pointer-events: none; }

    @media (max-width: 480px) {
      #music-player { bottom: 16px; right: 50%; transform: translateX(50%); }
      .polaroid-wrap { flex-wrap: nowrap; overflow-x: auto; padding-bottom: 16px; justify-content: flex-start; }
    }
```

- [ ] **Step 2: Replace `<div id="main-content"><!-- Populated in Task 2 --></div>` with the full content HTML**

```html
<div id="main-content">

  <!-- ── HERO ── -->
  <section id="hero">
    <div class="hero-top">Happy Birthday</div>
    <div class="hero-name" id="hero-name">Shreya</div>
    <div class="hero-badge">✨ Turning 20 · June 14th · With all my love 💕</div>
    <div class="scroll-arrow" id="scroll-arrow">↓</div>
  </section>

  <!-- ── LOVE LETTER ── -->
  <section id="letter">
    <div class="letter-wrap">
      <div class="section-label">A Letter For You</div>
      <div class="letter-icon">💌</div>
      <div class="gold-line"></div>
      <div class="letter-body">
        <p>Shreya, 20 years ago the world quietly became more beautiful without anyone quite noticing — except me.</p>
        <p>Every moment I've spent with you has felt like the first chapter of a story I never want to end. Your laugh, your kindness, the way you see the world — these are the things I carry with me everywhere I go.</p>
        <p>You have this rare ability to make ordinary moments feel like something out of a dream. The way you love, the way you care, the way you simply exist — it fills every room you walk into with something warmer than light.</p>
        <p>On this day, I want you to know that you are loved beyond every word I know how to say. Twenty years of you, and I still can't believe I get to be the person by your side.</p>
        <p class="letter-sign">Happy Birthday, jaan. — Always yours 💕</p>
      </div>
    </div>
  </section>

  <!-- ── 20 REASONS ── -->
  <section id="reasons">
    <div class="section-label">One For Every Year</div>
    <h2 class="section-title">20 Reasons I Love You</h2>
    <div class="gold-line"></div>
    <div class="reasons-grid" id="reasons-grid">
      <!-- injected by JS -->
    </div>
  </section>

  <!-- ── GALLERY ── -->
  <section id="gallery">
    <div class="section-label">Our Story</div>
    <h2 class="section-title">Us 💕</h2>
    <div class="gold-line"></div>
    <div class="polaroid-wrap" id="polaroid-wrap">
      <!-- injected by JS -->
    </div>
  </section>

  <!-- ── FOOTER ── -->
  <section id="footer">
    <p>Here's to forever with you 🌹</p>
    <p style="font-size:1rem; color: var(--rose-gold);">Turning 20 has never looked so beautiful.</p>
    <p class="footer-small">Made with love · June 14th, 2026</p>
  </section>

</div>

<!-- Floating music player -->
<div id="music-player" onclick="toggleMusic()">
  <span class="music-icon-el">🎵</span>
  <div class="music-info-wrap">
    <span class="music-title-el">Tum Hi Ho · Arijit Singh</span>
    <div class="music-bars" id="music-bars">
      <div class="music-bar"></div>
      <div class="music-bar"></div>
      <div class="music-bar"></div>
      <div class="music-bar"></div>
      <div class="music-bar"></div>
    </div>
  </div>
</div>

<!-- Hidden YouTube iframe — loaded on unlock -->
<iframe id="yt-frame" allow="autoplay"></iframe>
```

- [ ] **Step 3: Refresh browser, verify layout renders (main-content still hidden)**

The page should still show only the countdown. Open Dev Tools → Elements → confirm `#main-content` exists in DOM with `display:none`.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add all section HTML and CSS, content hidden until unlock"
```

---

### Task 3: Three.js particle system

**Files:**
- Modify: `shreya-birthday/index.html` — add Three.js setup in `<script>` block

- [ ] **Step 1: Add the Three.js particle scene script** (add before the closing `</body>` tag, after the existing `<script>`)

```html
<script>
// ── Three.js: Floating rose petal / heart particle field ──
let scene, camera, renderer, particles, particlePositions;
let burstActive = false;
const PARTICLE_COUNT = window.innerWidth < 768 ? 150 : 300;
const mouse = { x: 0, y: 0 };

function initThree() {
  const canvas = document.getElementById('bg-canvas');
  scene = new THREE.Scene();
  camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
  camera.position.z = 5;

  renderer = new THREE.WebGLRenderer({ canvas, alpha: true, antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

  // Create particle geometry
  const geometry = new THREE.BufferGeometry();
  const positions = new Float32Array(PARTICLE_COUNT * 3);
  const colors    = new Float32Array(PARTICLE_COUNT * 3);
  const sizes     = new Float32Array(PARTICLE_COUNT);

  // Store initial positions for drift animation
  particlePositions = [];

  for (let i = 0; i < PARTICLE_COUNT; i++) {
    const x = (Math.random() - 0.5) * 20;
    const y = (Math.random() - 0.5) * 20;
    const z = (Math.random() - 0.5) * 10;
    positions[i * 3]     = x;
    positions[i * 3 + 1] = y;
    positions[i * 3 + 2] = z;
    particlePositions.push({ x, y, z, speed: Math.random() * 0.4 + 0.1, offset: Math.random() * Math.PI * 2 });

    // Rose pink palette
    const t = Math.random();
    colors[i * 3]     = 0.8 + t * 0.2;   // R
    colors[i * 3 + 1] = 0.3 + t * 0.3;   // G
    colors[i * 3 + 2] = 0.5 + t * 0.3;   // B

    sizes[i] = Math.random() * 4 + 2;
  }

  geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
  geometry.setAttribute('color',    new THREE.BufferAttribute(colors, 3));
  geometry.setAttribute('size',     new THREE.BufferAttribute(sizes, 1));

  const material = new THREE.PointsMaterial({
    size: 0.12,
    vertexColors: true,
    transparent: true,
    opacity: 0.75,
    sizeAttenuation: true,
    blending: THREE.AdditiveBlending,
    depthWrite: false,
  });

  particles = new THREE.Points(geometry, material);
  scene.add(particles);

  window.addEventListener('resize', onResize);
  document.addEventListener('mousemove', (e) => {
    mouse.x = (e.clientX / window.innerWidth  - 0.5) * 2;
    mouse.y = (e.clientY / window.innerHeight - 0.5) * 2;
  });

  animateThree();
}

function animateThree() {
  requestAnimationFrame(animateThree);
  const t = Date.now() * 0.001;
  const pos = particles.geometry.attributes.position.array;

  for (let i = 0; i < PARTICLE_COUNT; i++) {
    const p = particlePositions[i];
    // Slow upward drift with sine wave side movement
    p.y += p.speed * 0.008;
    if (p.y > 10) p.y = -10; // wrap around

    pos[i * 3]     = p.x + Math.sin(t * p.speed + p.offset) * 0.5 - mouse.x * 0.3;
    pos[i * 3 + 1] = p.y + Math.cos(t * p.speed * 0.5 + p.offset) * 0.2 + mouse.y * 0.2;
    pos[i * 3 + 2] = p.z;
  }
  particles.geometry.attributes.position.needsUpdate = true;
  renderer.render(scene, camera);
}

function burstParticles() {
  // Shoot all particles outward from center then let them drift normally
  const pos = particles.geometry.attributes.position.array;
  for (let i = 0; i < PARTICLE_COUNT; i++) {
    const angle = Math.random() * Math.PI * 2;
    const radius = Math.random() * 8 + 2;
    particlePositions[i].x = Math.cos(angle) * radius;
    particlePositions[i].y = Math.sin(angle) * radius;
    particlePositions[i].z = (Math.random() - 0.5) * 6;
  }
}

function onResize() {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
}
</script>
```

- [ ] **Step 2: Verify Three.js loads (temporarily call `initThree()` at end of script and check for canvas)**

Add `initThree();` at end of the Three.js script block temporarily, refresh browser. The canvas should become visible with floating pink particles. Then remove `initThree();` from that location (it will be called properly in the unlock sequence in Task 4).

- [ ] **Step 3: Remove temporary `initThree()` call, commit**

```bash
git add index.html
git commit -m "feat: Three.js particle field with mouse parallax and burst"
```

---

### Task 4: Cinematic unlock sequence (GSAP timeline)

**Files:**
- Modify: `shreya-birthday/index.html` — add `triggerUnlock()` and confetti functions to `<script>`

- [ ] **Step 1: Add confetti spawner and unlock orchestration script** (add in a new `<script>` block before `</body>`)

```html
<script>
// ── Confetti ──
function spawnConfetti() {
  const colors = ['#ff6b9d','#f8a5c2','#ffd3e8','#c9965a','#fff','#ff4757'];
  for (let i = 0; i < 80; i++) {
    const el = document.createElement('div');
    el.className = 'confetti-piece';
    el.style.cssText = `
      left: ${Math.random() * 100}vw;
      background: ${colors[Math.floor(Math.random() * colors.length)]};
      width: ${Math.random() * 8 + 4}px;
      height: ${Math.random() * 8 + 4}px;
      border-radius: ${Math.random() > 0.5 ? '50%' : '2px'};
      animation-duration: ${Math.random() * 2 + 2}s;
      animation-delay: ${Math.random() * 1.5}s;
    `;
    document.body.appendChild(el);
    el.addEventListener('animationend', () => el.remove());
  }
}

// ── Populate reasons grid ──
const REASONS = [
  "Your smile that makes everything okay",
  "The way you laugh at your own jokes",
  "How kind you are to everyone around you",
  "Your strength — even when things are hard",
  "The way you make ordinary moments feel special",
  "Your eyes that say more than words",
  "How you always know what to say",
  "Your passion for everything you love",
  "The way you make me feel seen",
  "Your honesty, even when it's hard",
  "How you love with your whole heart",
  "Your voice — talking, singing, everything",
  "The little things you do without thinking",
  "How you make every place feel like home",
  "Your courage to be exactly yourself",
  "The way you care for people around you",
  "Your dreams — and how big they are",
  "How you've changed my world for the better",
  "Every memory we've made together",
  "Simply that you exist — and you're mine 💕"
];

function buildReasons() {
  const grid = document.getElementById('reasons-grid');
  REASONS.forEach((reason, i) => {
    const card = document.createElement('div');
    card.className = 'reason-card';
    card.innerHTML = `<span class="reason-num">${String(i+1).padStart(2,'0')}</span><span class="reason-text">${reason}</span>`;
    grid.appendChild(card);
  });
}

// ── Populate polaroid gallery ──
const PHOTOS = [
  { src: 'photos/photo-1.jpg', caption: 'Us 💕' },
  { src: 'photos/photo-2.jpg', caption: 'My favourite smile' },
  { src: 'photos/photo-3.jpg', caption: 'Always together' },
  { src: 'photos/photo-4.jpg', caption: 'You & me' },
  { src: 'photos/photo-5.jpg', caption: 'Perfect moments' },
  { src: 'photos/photo-6.jpg', caption: 'Forever like this' },
];

function buildGallery() {
  const wrap = document.getElementById('polaroid-wrap');
  PHOTOS.forEach(p => {
    const div = document.createElement('div');
    div.className = 'polaroid';
    div.innerHTML = `
      <div class="polaroid-img">
        <img src="${p.src}" alt="${p.caption}" onerror="this.style.display='none';this.parentElement.innerHTML='📸';" />
      </div>
      <div class="polaroid-caption">${p.caption}</div>
    `;
    wrap.appendChild(div);
  });
}

// ── Letter-by-letter hero name animation ──
function splitHeroName() {
  const el = document.getElementById('hero-name');
  const text = el.textContent;
  el.innerHTML = text.split('').map(ch =>
    `<span class="char" style="display:inline-block">${ch}</span>`
  ).join('');
}

// ── Music player ──
let musicPlaying = false;
function startMusic() {
  const iframe = document.getElementById('yt-frame');
  iframe.src = 'https://www.youtube.com/embed/Umqb9KENgmk?autoplay=1&mute=0&loop=1&playlist=Umqb9KENgmk&enablejsapi=1';
  musicPlaying = true;
}
function toggleMusic() {
  const iframe = document.getElementById('yt-frame');
  const bars = document.querySelectorAll('.music-bar');
  if (!musicPlaying) {
    startMusic();
    bars.forEach(b => b.classList.remove('paused'));
  } else {
    // Mute/unmute via postMessage — simplest approach for iframe
    iframe.contentWindow.postMessage('{"event":"command","func":"pauseVideo","args":""}', '*');
    musicPlaying = false;
    bars.forEach(b => b.classList.add('paused'));
  }
}

// ── Main unlock sequence ──
function triggerUnlock() {
  // Build dynamic content first
  buildReasons();
  buildGallery();
  splitHeroName();

  // Init Three.js
  initThree();

  const flash    = document.getElementById('flash');
  const locked   = document.getElementById('locked-screen');
  const bgCanvas = document.getElementById('bg-canvas');
  const main     = document.getElementById('main-content');
  const player   = document.getElementById('music-player');

  const tl = gsap.timeline();

  // 0.0s — freeze (already at 0)
  // 0.3s — flash
  tl.to(flash, { opacity: 1, duration: 0.15, ease: 'power2.in' }, 0.3)
    .to(flash, { opacity: 0, duration: 0.4, ease: 'power2.out' }, 0.45);

  // 0.5s — lock icon shatter (scale + rotate + fade)
  tl.to(locked.querySelector('.lock-icon'), {
    scale: 2, rotation: 30, opacity: 0, duration: 0.5, ease: 'back.in(2)'
  }, 0.5);

  // 0.8s — locked screen slides up and out
  tl.to(locked, {
    y: '-100%', opacity: 0, duration: 0.7, ease: 'power3.inOut',
    onComplete: () => { locked.style.display = 'none'; }
  }, 0.8);

  // 1.2s — Three.js canvas fades in + burst
  tl.to(bgCanvas, { opacity: 1, duration: 1, ease: 'power2.out',
    onStart: () => burstParticles()
  }, 1.2);

  // 1.5s — reveal main content
  tl.call(() => {
    main.style.display = 'block';
    main.style.opacity = '0';
  }, null, 1.5);
  tl.to(main, { opacity: 1, duration: 0.6, ease: 'power2.out' }, 1.6);

  // 2.0s — hero "Happy Birthday" label
  tl.to('.hero-top', { opacity: 1, y: 0, duration: 0.6, ease: 'power3.out' }, 2.0);

  // 2.2s — hero name letter by letter
  tl.to('#hero-name', { opacity: 1, duration: 0.01 }, 2.2);
  tl.from('#hero-name .char', {
    opacity: 0, y: 60, rotationX: -90, stagger: 0.08,
    duration: 0.6, ease: 'back.out(2)'
  }, 2.3);

  // 2.8s — confetti burst
  tl.call(() => spawnConfetti(), null, 2.8);

  // 3.0s — badge
  tl.to('.hero-badge', { opacity: 1, y: 0, duration: 0.5, ease: 'power2.out' }, 3.0);

  // 3.5s — scroll arrow
  tl.to('#scroll-arrow', { opacity: 1, duration: 0.4 }, 3.5);

  // 4.0s — music player appears + music starts
  tl.to(player, { opacity: 1, duration: 0.5, ease: 'power2.out',
    onStart: () => startMusic()
  }, 4.0);

  // 4.5s — register ScrollTrigger animations
  tl.call(() => registerScrollAnimations(), null, 4.5);
}

// ── ScrollTrigger reveal animations ──
function registerScrollAnimations() {
  gsap.registerPlugin(ScrollTrigger);

  // Letter paragraphs
  gsap.utils.toArray('.letter-body p').forEach((p, i) => {
    gsap.to(p, {
      opacity: 1, y: 0, duration: 0.9, ease: 'power3.out',
      scrollTrigger: { trigger: p, start: 'top 85%', toggleActions: 'play none none none' },
      delay: i * 0.05
    });
  });

  // Reasons cards — staggered cascade
  gsap.to('.reason-card', {
    opacity: 1, y: 0, duration: 0.7, ease: 'power3.out', stagger: 0.06,
    scrollTrigger: { trigger: '#reasons-grid', start: 'top 80%', toggleActions: 'play none none none' }
  });

  // Polaroids
  gsap.to('.polaroid', {
    opacity: 1, duration: 0.8, ease: 'power2.out', stagger: 0.12,
    scrollTrigger: { trigger: '#polaroid-wrap', start: 'top 80%', toggleActions: 'play none none none' }
  });
}
</script>
```

- [ ] **Step 2: Test unlock by temporarily changing `UNLOCK_DATE` to 1 second from now**

In the existing countdown script, temporarily change:
```js
const UNLOCK_DATE = new Date(Date.now() + 1000); // 1 second from now
```
Refresh the page. After ~1 second you should see:
- Screen flash white
- Lock icon spins and fades out
- Locked screen slides up
- Particles burst in
- "Happy Birthday" + "Shreya" animate in letter-by-letter
- Confetti falls
- Badge appears
- Music player slides in
- Scrolling reveals letter, reasons cards, polaroids

- [ ] **Step 3: Restore the real unlock date**

```js
const UNLOCK_DATE = new Date('2026-06-14T00:00:00');
```

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: cinematic unlock sequence with GSAP timeline and confetti"
```

---

### Task 5: Polish, mobile, README

**Files:**
- Modify: `shreya-birthday/index.html` — final polish
- Create: `shreya-birthday/README.md`

- [ ] **Step 1: Add final polish CSS** (add to the `<style>` block)

```css
    /* Smooth scroll on all sections */
    section { scroll-snap-align: start; }
    html { scroll-snap-type: y proximity; }

    /* Letter sign not a <p> tag fix */
    .letter-sign {
      font-family: 'Dancing Script', cursive !important;
      font-size: 1.6rem !important;
      color: var(--rose-pink) !important;
      margin-top: 16px !important;
    }

    /* Hero section vertical centering on short screens */
    @media (max-height: 600px) {
      #hero { min-height: auto; padding: 60px 24px; }
    }
```

- [ ] **Step 2: Verify on mobile viewport**

Open browser DevTools → toggle device toolbar → iPhone 12 (390×844). Check:
- Countdown tiles fit without overflow
- Hero name scales down (clamp works)
- Reasons grid is 2 columns
- Polaroids scroll horizontally
- Music player is centered at bottom

- [ ] **Step 3: Create README.md**

```markdown
# Happy Birthday Shreya 💕

A birthday website that unlocks at midnight on June 14th, 2026.

## How to personalize

### Swap your photos
Replace `photos/photo-1.jpg` through `photos/photo-6.jpg` with your actual photos.
- Recommended size: 400×400px square (will be cropped to square in polaroid)
- Any format works (jpg, png, webp)
- Update the captions in `index.html` → search `PHOTOS` array

### Edit the love letter
In `index.html`, search for `letter-body` and update the `<p>` tags inside.

### Edit the 20 reasons
In `index.html`, search for `const REASONS` and update the array strings.

### Change the unlock date
In `index.html`, search for `UNLOCK_DATE` and update:
```js
const UNLOCK_DATE = new Date('2026-06-14T00:00:00');
```

### Change the song
Replace the YouTube video ID `Umqb9KENgmk` in the `startMusic()` function.

## How to share with Shreya
Just send her the `index.html` file (she can open it in any browser),
or host it free on GitHub Pages / Netlify Drop (drag & drop the folder).
```

- [ ] **Step 4: Final full test with 1-second unlock**

Temporarily set `UNLOCK_DATE` to 1 second from now, do a full walkthrough:
1. Countdown ticks
2. Unlock sequence plays smoothly
3. Scroll through all sections
4. Verify reasons cascade in
5. Verify polaroids fade in (placeholders show 📸 emoji fallback)
6. Verify music player appears and clicking it loads YouTube
7. Restore real date

- [ ] **Step 5: Final commit**

```bash
git add index.html README.md
git commit -m "feat: complete birthday website — polish, mobile layout, README"
```

---

## Self-Review Checklist

| Spec Requirement | Covered in Task |
|-----------------|-----------------|
| Locked countdown screen | Task 1 |
| Countdown to June 14 12:00 AM | Task 1 |
| Cinematic unlock at midnight | Task 4 |
| Screen flash + lock shatters | Task 4 |
| Three.js 3D particles (rose petals / hearts) | Task 3 |
| Burst animation on unlock | Task 3 + 4 |
| Mouse parallax on particles | Task 3 |
| Hero: "Happy Birthday Shreya" letter-by-letter | Task 4 |
| Confetti burst | Task 4 |
| Love letter with scroll-reveal | Task 2 + 4 |
| 20 Reasons cascade animation | Task 2 + 4 |
| Polaroid photo gallery hover-tilt | Task 2 |
| Floating music player (Tum Hi Ho) | Task 2 + 4 |
| YouTube embed for audio | Task 4 |
| Mobile responsive | Task 5 |
| Single index.html, no build tools | All tasks |
| README for personalization | Task 5 |
