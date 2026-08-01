# [UNTITLED FARM GAME] — Design Doc

Working title needed. Placeholder: **FLOCK**.

Living document. Lives in the repo, committed like everything else.
Written before any code, deliberately — last project we found the real
shape halfway through and rebuilt around it twice.

---

## 1. The Pitch

Top-down pixel-art horde survival. You're a farmer. Waves of predators
come for your livestock. You have fists, then better things.

**The twist: you are not the objective.** The sheep is. And the sheep
has its own ideas — it wanders off, mid-wave, into the woods, and you
have to go and get it while everything else is still happening.

One sentence: *"Brotato, but you're also trying to keep a sheep alive
and the sheep is an idiot."*

### Why this works
- **Divided attention, not more enemies.** Panic comes from having
  three things to track (yourself, the flock, the pen) that pull in
  different directions — not from bigger numbers.
- **Emergencies, not a difficulty slope.** "THE PEN IS OPEN!" is a
  spike: sudden, specific, located. Those are the moments players
  remember and describe to other people.
- **Three upgrade axes for free:** weapons, flock management, pen
  integrity. Every shop visit is a real question.

### The moment the whole game exists to produce
You're at the pen fighting wolves off the fence. Behind you, unnoticed,
a second sheep slips through the open gate and heads for the treeline.
You hear it bleating somewhere in the woods.

---

## 2. Core Loop

1. Wave spawns; predators head for you, the sheep, and the pen
2. Fight, or don't — sometimes running is correct
3. Sheep wander. Alerts fire. You go and fetch
4. Wave ends → shop → next wave
5. Lose all sheep = run over

---

## 3. THE V1 BUILD (what gets made first)

**Scope is deliberately brutal. Nothing below this line is in v1.**

- ONE sheep
- ONE pen, with health, one gate
- ONE weapon (fists)
- ONE enemy type (wolf)
- THREE waves
- No shop, no upgrades, no hunger, no dog, no resources, no dens

**The only question this build answers:**
*Does running out through wolves to fetch a wandering sheep feel good?*

If yes, everything in the backlog is content on a proven foundation.
If no, we've lost a day instead of a week.

### v1 systems needed
- Top-down movement, camera follows player
- Y-sorting (see §5)
- Wolf AI: pick a target (player / sheep / pen), path toward it, attack
- Sheep AI: follow player loosely, graze, occasionally WANDER
- Pen: fixed structure, health, gate opens/closes on LMB when adjacent
  (gate highlights when you're in range — no aiming)
- Sheep returns to pen by walking over it and walking it home
- Bleat system (see §4)
- Wave spawner, wave counter, run-over state

---

## 4. The Bleat System

The sheep bleats periodically, more often when lost or frightened.

- **Volume scales with distance** from the player
- **Stereo pan** by direction — left/right positioning tells you which
  way to turn
- **A directional indicator** pops up at the screen edge pointing
  toward the sound, then fades

This is the navigation mechanic. Woods obscure vision; sound is how you
find your way back to the animal. Sound is doing gameplay work here,
not decoration.

---

## 5. Trees and Depth (top-down that doesn't look flat)

Two techniques, both cheap, both entirely in code — the ART STAYS FLAT.

**Y-sorting.** Everything on the map is drawn in order of its base Y
coordinate. Lower on screen = drawn later = appears in front. Walk
below a tree, you're in front of it; walk above it, you're behind.

**Canopy separation.** A tree is TWO images: a trunk (Y-sorted like any
object, blocks movement) and a canopy (always drawn on top, goes
semi-transparent when the player is under it). Standing in woodland
means leaves overhead and genuinely obscured vision — fog of war for
free, and the reason the bleat matters.

---

## 6. Open Design Questions (decide before they're built)

- **Why leave the pen?** If camping the gate is optimal, the map is
  decoration. Current answer: sheep wander often enough that camping
  loses sheep anyway. BETTER answer for v2: resources scattered on the
  map, so leaving is rewarded rather than merely forced.
- **Carrying the sheep?** Pick it up, move slower, can't fight? Strong
  decision-generator. Not in v1 — decide after playtest.
- **Does the pen protect, or just delay?** Currently: fence has health,
  doesn't heal between waves. Repair is an upgrade.
- **Tone.** Undecided and deliberately so. This can be funny, tense or
  bleak. NOT assumed to be a comedy.

---

## 7. Backlog (agreed as good, explicitly NOT v1)

Rough priority order.

- Shop + upgrades between waves (three axes: weapon / flock / pen)
- Multiple sheep (2, 3, 4...) — flock size as difficulty and as score
- Weapons: rake, pickaxe, axe, shotgun
- Pen upgrades: stronger fence, hammer-and-nails repair, bigger pen
- **Hunger** — feed depletes, more starting feed is an upgrade
- **Resources** scattered on the map, spent on upgrades
- **Sheepdog** — herds strays back, scares predators, has its own health
- **Dens** at the map edge that spawn predators; destroy to reduce
  pressure
- More predators: fox, boar, eventually a dragon
- Chickens, and more of the farm to defend
- Day/night, weather

---

## 8. Art Spec

### The rule
**Draw at 1× in Aseprite, export at 2×. The game draws the exported
file 1:1.** So a 24×24 drawing becomes a 48×48 PNG and appears 48px on
screen. Chunky 2px-per-pixel, consistent with everything else.

Image smoothing is off in code, so nothing blurs.

### Directions — read this before drawing anything
- Draw **THREE** directions only: **down (facing camera), up (facing
  away), side (facing right)**.
- **Left is a horizontal flip of right, done in code.** Don't draw it.
  That's a third of the work gone.
- **Death does NOT need a direction.** One death animation per creature,
  reused for all facings. Standard practice, big saving.
- **Attacks are not needed in v1.** The lunge is code-driven movement.
  Add attack frames later if the game wants them.

### Export format
One PNG per animation, frames laid out **left to right in a horizontal
strip**. Aseprite: File → Export Sprite Sheet → Horizontal Strip.

So `wolf-walk-side.png` with 4 frames of 48×48 = a 192×48 file. The
code slices it by frame count.

### v1 ASSET LIST — characters

| File | Draw | Export | Frames | Notes |
|---|---|---|---|---|
| farmer-idle-down.png | 24×24 | 48×48 | 2 | gentle breathing |
| farmer-idle-up.png | 24×24 | 48×48 | 2 | |
| farmer-idle-side.png | 24×24 | 48×48 | 2 | |
| farmer-walk-down.png | 24×24 | 48×48 | 4 | |
| farmer-walk-up.png | 24×24 | 48×48 | 4 | |
| farmer-walk-side.png | 24×24 | 48×48 | 4 | |
| sheep-idle-down.png | 20×20 | 40×40 | 2 | |
| sheep-idle-up.png | 20×20 | 40×40 | 2 | |
| sheep-idle-side.png | 20×20 | 40×40 | 2 | |
| sheep-walk-down.png | 20×20 | 40×40 | 4 | |
| sheep-walk-up.png | 20×20 | 40×40 | 4 | |
| sheep-walk-side.png | 20×20 | 40×40 | 4 | |
| sheep-graze.png | 20×20 | 40×40 | 3 | head down, munching |
| wolf-walk-down.png | 24×24 | 48×48 | 4 | |
| wolf-walk-up.png | 24×24 | 48×48 | 4 | |
| wolf-walk-side.png | 24×24 | 48×48 | 4 | |
| wolf-death.png | 24×24 | 48×48 | 4 | one only, all directions |

### v1 ASSET LIST — world

| File | Draw | Export | Notes |
|---|---|---|---|
| grass-tile.png | 32×32 | 64×64 | must tile seamlessly — edges match |
| tree-trunk.png | 24×32 | 48×64 | solid, blocks movement, Y-sorted |
| tree-canopy.png | 48×40 | 96×80 | drawn over everything, fades when under |
| fence-h.png | 16×16 | 32×32 | horizontal run, tileable |
| fence-v.png | 16×16 | 32×32 | vertical run, tileable |
| fence-corner.png | 16×16 | 32×32 | |
| gate-closed.png | 32×16 | 64×32 | |
| gate-open.png | 32×16 | 64×32 | |
| props.png | 16×16 each | 32×32 | strip of scatter: flower, litter, a shoe, a rock, a bush |

Optional but cheap and adds a lot: a second tree variant, a dirt-patch
tile, a puddle.

### v1 ASSET LIST — audio

| File | Notes |
|---|---|
| sfx-bleat-calm-1.wav / -2 | Contented. LOW, SLOW, unhurried — heard every 6–15s when it's safe. Must not grate |
| sfx-bleat-panic-1.wav / -2 / -3 | Frightened. Higher, shorter, sharper — fires every ~1.5s when a wolf is near or loose in the pen. This is the one that means "GO NOW" |
| sfx-punch.wav | |
| sfx-hit.wav | connecting with a wolf |
| sfx-wolf-growl.wav | |
| sfx-wolf-die.wav | |
| sfx-fence-hit.wav | wood taking damage |
| sfx-gate.wav | open/close |
| sfx-wave-start.wav | |
| music-intro.wav | 3-bar intro, plays once at the start |
| music-loop.wav | main loop — must join seamlessly from the end of the intro |

### Craft notes
- **Pick a palette first and stay in it.** 12–16 colours for the whole
  game. This is what makes a set of sprites look like one game rather
  than a collection. Aseprite has palette presets; a pastoral/earthy one
  suits this.
- **Silhouette test:** at 48px, squint. Farmer, sheep and wolf must be
  distinguishable by shape alone, with no colour. If the wolf reads as
  the sheep at a glance, redraw.
- **Sheep must be the most readable thing on screen.** It's the thing
  you're constantly looking for. Bright, high contrast against grass.
- **The wolf should read as a threat at a glance** — low, dark,
  pointed. It'll usually be seen in motion at the screen edge.
- **Grass tile: keep it quiet.** Low contrast, minimal detail. Busy
  ground makes a lost sheep impossible to spot and the whole screen
  noisy.
- Filenames exact, lowercase, as listed. Missing files fall back to
  coloured rectangles, no errors.

---

## 9. Playtest 1 — findings (designer)

The loop works. "Eight wolves chasing my sheep through the trees and me
chasing after them" — emergent, unscripted, and funny. Foundation holds.

Acted on:
- **Gate moved to RMB / E / pad-B.** Punching at the gate was opening it
  by accident — same button did both. LMB / SPACE / pad-A now swings only.
  Hold-RMB-to-walk removed; WASD won on feel, and RMB was needed for the gate.
- **Woods roughly doubled and CLUMPED** (90 scattered → ~158 in 16 clusters
  plus loners). Real woods and real clearings instead of even scatter.
  9% of the map is now solid, so it's still traversable.
- **Bleat split into calm and panic.** Was constant and grating. Calm is
  slow and occasional; panic only fires when a wolf is within 260px or one
  is loose inside the pen.
- **False "WANDERED OFF" alert fixed.** It fired whenever the sheep picked
  a wander target, even standing in the pen. Now it fires only on the frame
  it actually crosses out of the pen.

Added on the back of it:
- **SHOO command (Q / gamepad X).** Stops the sheep trailing you. Said
  inside the pen it also SETTLES the animal — it won't wander or re-latch
  for 12–23s, and shows a small "staying" tag. Farmer shouts
  "GEEET BACK IN THERE" (or "STAY THERE THEN" outside the pen).
- **It only follows you out of the pen ~50% of the time.** Measured at 46%
  over 200 trials. Leaving now carries a small risk you have to glance back
  and check, rather than a guaranteed escort.

Confirmed good, left alone: movement speed, canopy fade, the gate being a
genuinely dangerous thing to open. Punching is "fine, not satisfying" —
noted, not yet addressed.

## 10. Decisions Log

- 2026-07: Genre chosen — real-time top-down pixel horde survival.
  Turn-based parked.
- 2026-07: **THE TWIST (designer):** protect a wandering animal, not
  just yourself. Farmer + sheep + predators.
- 2026-07: Pen added — gives the map a centre and a thing to defend.
- 2026-07: Gate is LMB when adjacent, highlights in range. No aiming.
- 2026-07: Losing ALL sheep ends the run (not the first one).
- 2026-07: **Bleat navigation (designer)** — distance-scaled volume,
  stereo pan, edge indicator. Sound as a gameplay system.
- 2026-07: Trees = trunk + canopy, Y-sorted, canopy fades when under.
- 2026-07: v1 scope frozen at 1 sheep / 1 pen / fists / 1 enemy /
  3 waves, no shop. Everything else backlogged.
- 2026-07: Tone deliberately undecided. NOT assumed comedy.
- 2026-07: Controls settled — WASD/arrows/stick move, LMB/SPACE/A swing,
  RMB/E/B gate, Q/pad-X shoo. All schemes live at once, no menu toggle.
- 2026-07: Herding is not sticky — a herded sheep only trails you out of
  the pen about half the time, and SHOO releases it deliberately.
- 2026-07: AI steering added (fan out around obstacles) plus a stuck-wolf
  failsafe — a wolf wedged behind scenery meant a wave that could never
  end, which is worse than a wolf clipping a tree.