# Happy Birthday Shreya 💕

A birthday website that **locks until midnight on June 14th, 2026** — then dramatically reveals everything with a cinematic animation.

## How it works

- **Before June 14th 12:00 AM** → Shreya sees a mysterious countdown: *"Something beautiful is waiting for you…"*
- **At midnight on June 14th** → The site unlocks with a screen flash, the lock shatters, rose petal particles burst, confetti falls, her name animates letter-by-letter, and Tum Hi Ho starts playing
- **After unlock** → Full scrollable experience: love letter, **Our Story timeline**, 20 reasons, **Her Name Explained** (S·H·R·E·Y·A acrostic), photo gallery, **Love Coupons**, birthday cake, **grand finale** (press & hold the heart!), and a **P.S. that types itself**
- **Live "days of you" counter** in the hero — ticks every second since her birth (edit `BIRTH_DATE` in index.html if her birth time isn't midnight)
- **Hidden secret** 🤫 — tapping "Here's to forever with you" in the footer three times reveals a secret message (edit `.secret-text` in index.html to make it yours)
- Backups: `index-v1-classic.html` (pink original without new sections) · `index-v2-velvet.html` (gold/velvet redesign)

## 🔑 Preview mode (for YOU, before her birthday)

The page is locked until June 14th — but you can preview everything right now by opening:
```
index.html?preview
```
(just add `?preview` to the end of the URL). **Don't share the preview link with her** — give her the normal link so she gets the midnight reveal.

## How to personalise

### Add your photos
Put your photos in the `photos/` folder named exactly:
```
photos/photo-1.JPG ... photos/photo-6.JPG
```
⚠️ **Important — file extension must match the code.** The site currently looks for `.JPG` (uppercase). On your Mac it works either way, but once hosted online (Netlify/GitHub Pages) the name must match EXACTLY, including case.

⚠️ **iPhone photos:** if a photo doesn't show up, it's probably a HEIC file renamed to .JPG. Convert it first:
```
sips -s format jpeg your-photo.heic --out photo-4.JPG
```
Slots 4 and 6 currently show a pretty "A memory of us goes here 💕" placeholder until you add those photos.

To change the captions, find `var PHOTOS = [` in `index.html` and update each `caption` value.

### Edit the Our Story timeline
Find `var TIMELINE = [` in `index.html` — replace the placeholder milestones with your real dates and memories (the day you met, first date, etc.).

### Edit the Love Coupons
Find `var COUPONS = [` in `index.html` — six promises she can tap to redeem. Make them yours!

### Edit the love letter
In `index.html`, find the `<div class="letter-body">` section and edit the `<p>` paragraphs inside it.

### Edit the 20 reasons
In `index.html`, find `var REASONS = [` and update any of the 20 strings in the array.

### Change the song
In `index.html`, find `startMusic()` and replace `Umqb9KENgmk` with any YouTube video ID.

### Change the unlock date/time
In `index.html`, find:
```js
var UNLOCK_DATE = new Date('2026-06-14T00:00:00');
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
