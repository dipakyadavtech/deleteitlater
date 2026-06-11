# Graph Report - .  (2026-06-11)

## Corpus Check
- 9 files · ~225,964 words
- Verdict: corpus is large enough that graph structure adds value.

## Summary
- 67 nodes · 72 edges · 9 communities (7 shown, 2 thin omitted)
- Extraction: 68% EXTRACTED · 26% INFERRED · 6% AMBIGUOUS · INFERRED: 19 edges (avg confidence: 0.82)
- Token cost: 198,338 input · 0 output

## Community Hubs (Navigation)
- [[_COMMUNITY_Unlock Animation Sequence|Unlock Animation Sequence]]
- [[_COMMUNITY_Countdown & Core State|Countdown & Core State]]
- [[_COMMUNITY_Childhood Studio Portrait|Childhood Studio Portrait]]
- [[_COMMUNITY_Couple Selfie (Carved Wood)|Couple Selfie (Carved Wood)]]
- [[_COMMUNITY_Couple Selfie (Yellow Walls)|Couple Selfie (Yellow Walls)]]
- [[_COMMUNITY_Couple Selfie (White Walls)|Couple Selfie (White Walls)]]
- [[_COMMUNITY_Three.js Particle Engine|Three.js Particle Engine]]
- [[_COMMUNITY_Photo Gallery & Lightbox|Photo Gallery & Lightbox]]
- [[_COMMUNITY_Local Dev Server Config|Local Dev Server Config]]

## God Nodes (most connected - your core abstractions)
1. `triggerUnlock() master sequence` - 14 edges
2. `Shreya's Birthday Website Design Spec` - 6 edges
3. `Young Girl Subject` - 6 edges
4. `Selfie of a Young Couple Indoors` - 4 edges
5. `Young Woman with Long Dark Hair in Blue T-Shirt` - 4 edges
6. `Close-Up Couple Selfie Indoors` - 4 edges
7. `Couple Posing Together` - 4 edges
8. `Childhood Studio Portrait Photograph` - 4 edges
9. `Locked Countdown State` - 3 edges
10. `Cinematic Midnight Unlock Sequence` - 3 edges

## Surprising Connections (you probably didn't know these)
- `triggerUnlock() master sequence` --implements--> `Cinematic Midnight Unlock Sequence`  [INFERRED]
  index.html → docs/superpowers/specs/2026-05-23-shreya-birthday-design.md
- `updateCountdown()` --implements--> `Locked Countdown State`  [INFERRED]
  index.html → docs/superpowers/specs/2026-05-23-shreya-birthday-design.md
- `startMusic()` --implements--> `Tum Hi Ho music via hidden YouTube iframe`  [INFERRED]
  index.html → docs/superpowers/specs/2026-05-23-shreya-birthday-design.md
- `Shreya Birthday Website Implementation Plan` --references--> `README: Happy Birthday Shreya website`  [EXTRACTED]
  docs/superpowers/plans/2026-05-23-shreya-birthday.md → README.md
- `Shreya Birthday Website Implementation Plan` --references--> `Shreya's Birthday Website Design Spec`  [INFERRED]
  docs/superpowers/plans/2026-05-23-shreya-birthday.md → docs/superpowers/specs/2026-05-23-shreya-birthday-design.md

## Import Cycles
- None detected.

## Hyperedges (group relationships)
- **Midnight unlock orchestration: countdown triggers timeline which builds content, inits 3D, and starts music** — index_updatecountdown, index_triggerunlock, index_initthree, index_registerscrollanimations, index_startmusic [EXTRACTED 0.85]
- **Three.js particle field: init, heart texture, animation loop, burst, and SHREYA text formation** — index_initthree, index_createhearttexture, index_animthree, index_burstparticles, index_formtextparticles [EXTRACTED 0.80]
- **Design spec drives implementation plan which produces README and index.html artifact** — design_shreya_birthday_spec, plan_shreya_birthday, readme_birthday_website [INFERRED 0.75]

## Communities (9 total, 2 thin omitted)

### Community 0 - "Unlock Animation Sequence"
Cohesion: 0.14
Nodes (8): buildReasons(), burstParticles(), formTextParticles() spell SHREYA, REASONS data array (20 reasons), registerScrollAnimations(), spawnCakeConfetti(), spawnConfetti(), triggerUnlock() master sequence

### Community 1 - "Countdown & Core State"
Cohesion: 0.18
Nodes (11): All content in DOM at load, hidden via display:none until unlock, Locked Countdown State, Single index.html, no build tools (CDN-only), Tum Hi Ho music via hidden YouTube iframe, Cinematic Midnight Unlock Sequence, Shreya's Birthday Website Design Spec, startMusic(), UNLOCK_DATE constant (+3 more)

### Community 2 - "Childhood Studio Portrait"
Cohesion: 0.22
Nodes (10): Bindi and Forehead Tilak, Gold Necklaces, Nostalgic Childhood Memory Mood, Pink Friend Print Top, Printed Fabric Surface, Childhood Studio Portrait Photograph, Shreya (Likely Subject), Studio Passport-Style Photo (+2 more)

### Community 3 - "Couple Selfie (Carved Wood)"
Cohesion: 0.22
Nodes (9): Ornate Carved Dark Wood Interior, Couple Posing Together, Patterned Ethnic Top and Jhumka Earrings, Grey Graphic T-Shirt, Smiling Bearded Man Wearing Glasses, Indoor Selfie of a Couple, Shreya (Birthday Subject), Warm Affectionate Mood (+1 more)

### Community 4 - "Couple Selfie (Yellow Walls)"
Cohesion: 0.38
Nodes (7): Affectionate Couple / Close Relationship, Indoor Room with Yellow Walls, Smiling Bearded Man with Round Glasses, Happy, Warm, Intimate Mood, Selfie of a Young Couple Indoors, Shreya (Birthday Subject), Young Woman with Long Dark Hair in Blue T-Shirt

### Community 5 - "Couple Selfie (White Walls)"
Cohesion: 0.38
Nodes (7): Warm Affectionate Candid Mood, Gray Hoodie with Text Print, Indoor Setting with White Walls, Decorative Metal Grille Window, Close-Up Couple Selfie Indoors, Smiling Young Man with Round Glasses, Young Woman in Selfie

### Community 7 - "Photo Gallery & Lightbox"
Cohesion: 1.00
Nodes (3): buildGallery(), openLightbox(), PHOTOS data array (6 polaroids)

## Ambiguous Edges - Review These
- `Young Woman with Long Dark Hair in Blue T-Shirt` → `Shreya (Birthday Subject)`  [AMBIGUOUS]
  photos/photo-1.JPG · relation: semantically_similar_to
- `Young Woman in Selfie` → `Warm Affectionate Candid Mood`  [AMBIGUOUS]
  photos/photo-2.JPG · relation: conceptually_related_to
- `Smiling Young Woman with Long Dark Hair` → `Shreya (Birthday Subject)`  [AMBIGUOUS]
  photos/photo-3.JPG · relation: semantically_similar_to
- `Young Girl Subject` → `Shreya (Likely Subject)`  [AMBIGUOUS]
  photos/photo-5.JPG · relation: semantically_similar_to

## Knowledge Gaps
- **20 isolated node(s):** `shreya-birthday launch config (python http.server :7771)`, `README: Happy Birthday Shreya website`, `UNLOCK_DATE constant`, `REASONS data array (20 reasons)`, `Shreya (Birthday Subject)` (+15 more)
  These have ≤1 connection - possible missing edges or undocumented components.
- **2 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **What is the exact relationship between `Young Woman with Long Dark Hair in Blue T-Shirt` and `Shreya (Birthday Subject)`?**
  _Edge tagged AMBIGUOUS (relation: semantically_similar_to) - confidence is low._
- **What is the exact relationship between `Young Woman in Selfie` and `Warm Affectionate Candid Mood`?**
  _Edge tagged AMBIGUOUS (relation: conceptually_related_to) - confidence is low._
- **What is the exact relationship between `Smiling Young Woman with Long Dark Hair` and `Shreya (Birthday Subject)`?**
  _Edge tagged AMBIGUOUS (relation: semantically_similar_to) - confidence is low._
- **What is the exact relationship between `Young Girl Subject` and `Shreya (Likely Subject)`?**
  _Edge tagged AMBIGUOUS (relation: semantically_similar_to) - confidence is low._
- **Why does `triggerUnlock() master sequence` connect `Unlock Animation Sequence` to `Countdown & Core State`, `Three.js Particle Engine`, `Photo Gallery & Lightbox`?**
  _High betweenness centrality (0.198) - this node is a cross-community bridge._
- **Why does `Cinematic Midnight Unlock Sequence` connect `Countdown & Core State` to `Unlock Animation Sequence`?**
  _High betweenness centrality (0.054) - this node is a cross-community bridge._
- **Are the 2 inferred relationships involving `Young Woman with Long Dark Hair in Blue T-Shirt` (e.g. with `Affectionate Couple / Close Relationship` and `Smiling Bearded Man with Round Glasses`) actually correct?**
  _`Young Woman with Long Dark Hair in Blue T-Shirt` has 2 INFERRED edges - model-reasoned connections that need verification._