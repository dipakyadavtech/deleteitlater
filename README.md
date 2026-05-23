# Happy Birthday Shreya 💕

A birthday website that **locks until midnight on June 14th, 2026** — then dramatically reveals everything with a cinematic animation.

## How it works

- **Before June 14th 12:00 AM** → Shreya sees a mysterious countdown: *"Something beautiful is waiting for you…"*
- **At midnight on June 14th** → The site unlocks with a screen flash, the lock shatters, rose petal particles burst, confetti falls, her name animates letter-by-letter, and Tum Hi Ho starts playing
- **After unlock** → Full scrollable experience: love letter, 20 reasons, photo gallery, music player

## How to personalise

### Add your photos
Put your photos in the `photos/` folder named exactly:
```
photos/photo-1.jpg
photos/photo-2.jpg
photos/photo-3.jpg
photos/photo-4.jpg
photos/photo-5.jpg
photos/photo-6.jpg
```
Recommended: square images (400×400px or larger). Any format works (jpg, png, webp).

To change the captions, find `var PHOTOS = [` in `index.html` and update each `caption` value.

### Edit the love letter
In `index.html`, find the `<div class="letter-body">` section and edit the `<p>` paragraphs inside it.

### Edit the 20 reasons
In `index.html`, find `var REASONS = [` and update any of the 20 strings in the array.

### Change the song
In `index.html`, find `startMusic()` and replace `Umqb9KENgmk` with any YouTube video ID.

### Change the unlock date/time
In `index.html`, find:
```js
const UNLOCK_DATE = new Date('2026-06-14T00:00:00');
```
Update to any date and time you want.

## How to share with Shreya

**Option 1 — Send the file directly**
Just send her the `index.html` file (WhatsApp, email, Google Drive). She can open it in any browser on her phone or laptop — no internet needed (except for fonts and music).

**Option 2 — Host it free online (recommended)**
1. Go to [Netlify Drop](https://app.netlify.com/drop)
2. Drag and drop the entire `shreya-birthday` folder
3. Netlify gives you a link instantly — share it with her!

**Option 3 — GitHub Pages**
Push this folder to a GitHub repo and enable Pages in Settings → Pages.

## Tech used
Three.js · GSAP + ScrollTrigger · Google Fonts · YouTube embed · Vanilla JS · No build tools needed
