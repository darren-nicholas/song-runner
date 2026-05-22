---
tags: [project, game, iphone, design]
status: active
---

# Song Runner — Kickoff Prompt (Design Source of Truth)

[[Projects]]

> Working title: Song Runner. The core idea: **the song IS the level.**

---

## The Core Design Pillar

Each level is built around one ~3-minute instrumental track. Gameplay events — enemy waves, hazards, day/night visual shifts, difficulty ramps — are mapped to timestamps in the song. Survive to the final note and you beat the level. The whole engine keys off the audio's current playback position.

## Direction & Perspective
**Left-to-right side scroller. Alto's Adventure visual style.**
- World scrolls right-to-left, player car fixed at ~25% from left edge
- Camera is side-on — full sky visible above road at all times
- Road is a single surface line (not a band) — car rides it or jumps above it
- Sky above is open — no ceiling. Gravity brings car back to road.
- Road rolls over hills, creating natural vertical movement of the surface

## Visual Style — Alto's Adventure as reference
- Parallax sky layers: gradient sky, distant mountains, mid-ground trees/buildings
- Atmospheric lighting: time-of-day shifts (dawn/day/dusk/night) affect sky color + silhouette mood
- Silhouetted scenery — trees, buildings as dark shapes against the sky
- Car shadow on road surface when airborne (shows height, critical for landing timing)
- Music-timed ramps → big air moments silhouetted against the sky

## Core Mechanic — Pure Jump Evasion
- **Jump is the only evasion move** — no left/right swerving, no lanes
- Jump over low threats (oncoming cars, craters, debris)
- Stay low for high threats (bridges, aerial rockets)
- Timed jumps at music ramps = stylish moments (visual payoff, score bonus)
- Landing shadow guides timing

## Threat Model
- **From the right (ahead)** — oncoming cars, obstacles, craters → jump over
- **From the left (behind)** — chasers closing in → rear weapons
- **Aerial** — rockets, low bridges → stay grounded
- **Road craters** — persistent obstacles created by explosions → must jump

## Weapons
- **Collected as road pickups** — drive over to grab
- **All rear-deployed** — smoke screen (slows chasers), tack strip (spins out permanently)
- Weapon type shown in HUD, deployed on button press

## Controls (final)
- **Jump** — Space / tap upper screen
- **Deploy rear weapon** — S / tap lower-left
- **Shoot ahead** — auto-fire or button (TBD)
- No lane-switching, no swerving

## What's Proven in the Prototype (v1 — being superseded)

- **Timeline-driven engine**: a `TIMELINE` array of `{ t: seconds, type, ... }` events that fire as song-time advances. This array IS the level design.
- Clock reads `audio.currentTime` when a track is loaded, with synthetic fallback clock for testing.
- 4-lane road, smooth lane-switching, retro CRT/scanline aesthetic.
- Health system: 3 HP, post-hit invulnerability shield, health pickups via shootable crates and road items.
- Day→dusk→night→dawn phase shifts mapped to the timeline.

## Tech Direction (Decided)

- **Web-first.** HTML5 Canvas + vanilla JS (or light tooling).
- **Ship to iOS via Capacitor** — do NOT rebuild native. Keep web-deployable throughout.
- Deploy via GitHub → Vercel for web builds.
- **Known risk:** iOS audio sync and autoplay in WKWebView. De-risk early — music sync IS the gameplay.

## Full Scope (eventual, not all Phase 1)

- 3 levels = 3 songs + 3 roadside art themes (city / country / desert)
- Real pixel art replacing placeholder colored boxes
- Menu → level select → play → win/lose → retry loop
- "Juice": screen shake, hit flashes, sound effects
- **Monetization (LATER):** free first level as hook, one-time IAP ~$3.99 to unlock rest, cosmetic car pack as second SKU. One-time purchases only — no subscriptions, no coin economy, no ads at launch.

## Music

4 tracks created in ElevenLabs. Currently in vault at:
`02-Projects/games/Road Warrior/music/`
- `Asphalt_Serpent.mp3`
- `Midnight_Prowler_1.mp3`
- `Midnight_Prowler_2.mp3`

## Working Prototype

Located at: `02-Projects/games/Road Warrior/index.html`
Basic proof of concept — pseudo-3D road, lane switching, enemies, shooting, smoke, oil slick, music-driven level end.

## Current Prototype State (as of 2026-05-22)

File: `02-Projects/games/Road Warrior/song-runner-sidescroll.html`

**Working:**
- Smoothstep terrain with flat sections at peaks/valleys
- Reference-model gravity (terrain drops → car floats naturally, no launch calculation)
- Manual jump (Space) + double jump
- Car tilts with terrain slope (amplified 2.2×, holds briefly when airborne)
- Up arrow throttle — hold to build speed up to 1.6×, bleeds off on release
- Chasers visible on screen, advance with world + close in on player
- Chaser bullets stop at rising terrain
- Traffic cars go same direction (player overtakes from behind)
- Forward shooting (Space also fires)
- Rear weapon deployment (S key)
- Smoke + tack strip pickups
- Speed boost pickup (gold orb)
- Rockets with telegraph→strike pattern
- Day/dusk/night/dawn phase shifts
- Parallax sky: clouds, mountains, far trees, near trees/buildings
- Timeline-driven events off audio.currentTime
- Music auto-loads from music/ folder

**Open threads:**
- Screen shake (landing, hits, rocket strikes) — next GitHub search
- Parallax layers could be richer
- Title still TBD — atmospheric/cinematic direction
- All graphics are placeholder boxes — pixel art is future phase
- Capacitor wrap for iOS — future phase

## How to Work (from kickoff prompt)

1. Read prototype → reflect architecture back
2. Propose clean project structure + tooling tradeoffs
3. Propose Phase 1 scope (smallest genuinely fun full loop, one polished level)
4. Build incrementally — show what changed + how to run after each chunk
5. Keep tuning knobs (HP, shield duration, reaction windows, spawn rates, speeds) in one obvious place
