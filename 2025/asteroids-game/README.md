# Asteroids Game

A Canvas 2D browser implementation of Asteroids with three progression levels, procedural asteroid rendering, and a dual-canvas architecture separating the animated starfield from the game layer.

## Overview

The game runs across two `<canvas>` elements stacked by CSS z-index. A background canvas renders an independently-ticking starfield and the foreground canvas handles all game objects. `requestAnimationFrame` drives a single `gameLoop` that calls `drawStars`, `update`, and `render` on every tick.

## Core Mechanisms

### State Machine

A `gameState` object holds `lives`, `score`, `level`, `gameOver`, `won`, `transitioning`, and `transitionTimer`. Level advancement is gated by `transitioning`: when the asteroid array empties, `advanceToNextLevel` sets `transitioning = true` and `transitionTimer = 180` (three seconds at 60 fps). While `transitioning` is true, `update` returns immediately after decrementing the timer, preventing input and collision logic from running during the transition overlay.

### Asteroid Generation

`createAsteroids` is parameterised by `gameState.level`:

| Level | Count | Size | Base speed |
|-------|-------|------|------------|
| 1     | 5     | 40   | 2          |
| 2     | 7     | 35   | 3          |
| 3     | 9     | 30   | 3.5        |

Each asteroid is placed at a random position that satisfies a minimum Euclidean distance of 150 px from the ship, preventing immediate collision on spawn. Colour hue is sampled from level-specific HSL ranges: amber–orange for level 1, cyan–blue for level 2, and magenta–purple for level 3.

### Asteroid Rendering — Per-Level Shape Types

`drawAsteroid` branches on `asteroid.shapeType`, which equals the level number at creation and is preserved through fragmentation.

**Type 1 (10 vertices):** Radius modulated by `sin(i × 2.7)`. A radial gradient shifts the HSL lightness from 70% at the highlight to 20% at the edge, with a semi-transparent shadow rendered 3 px offset before the main fill.

**Type 2 (8 vertices):** Radius modulated by `sin(i × 3)`, producing a more star-like silhouette. A three-stop gradient runs to 1.2× the nominal radius. Chord lines connecting opposite vertices at 60% radius provide an internal crystal structure.

**Type 3 (12 vertices):** Radius modulated by `sin(i × 4)`. A 13-stop-equivalent gradient extends to 1.3× radius. Four evenly-spaced vertices carry filled arcs at 30% of local radius, and cross-chord lines at 25%-step intervals add further internal detail. Stroke weight is 4 px, giving the level-3 asteroids a visually heavier appearance.

### Fragmentation

`breakAsteroid` halves the asteroid's size and pushes two children if the original size exceeds 15 px. Each child inherits 80% of the parent's velocity plus random perturbation, and retains the parent's `shapeType` and `color`. This preserves the visual language of each level through the full fragmentation tree. `createParticles` then emits 15 radially-distributed particles at the impact point.

### Physics and Wrapping

Ship thrust accumulates into `vx`/`vy` at 0.25 px per frame and is attenuated by a per-frame multiplier of 0.99, producing asymptotic deceleration. The `wrap` function clamps all objects to the canvas boundaries by teleporting them to the opposite edge. Projectiles carry a `life` counter of 80 frames and a trailing eight-point path stored in `missile.trail`, rendered as a tapered stroke.

### Scoring

Points are awarded on missile–asteroid collision and scale by both `shapeType` and remaining fragment size:

| Size class | Type 1 | Type 2 | Type 3 |
|-----------|--------|--------|--------|
| Large (>30) | 20   | 40     | 80     |
| Medium (>15)| 50   | 100    | 200    |
| Small       | 100  | 200    | 400    |

### Invulnerability

After a life is lost, `resetShip` sets `invulnerable = 120` (two seconds). During this window, `checkCollision` against asteroid–ship pairs is bypassed. The ship sprite is rendered using a `Math.floor(invulnerable / 5) % 2` blink pattern, toggling visibility every five frames.

### Starfield

`initStars` generates 200 star objects with random position, size (0–2 px), and brightness. On each tick, `drawStars` paints a near-transparent fill (`rgba(10, 10, 21, 0.1)`) over the star canvas, leaving the previous frame partially visible and producing a slow fade trail. Each star's brightness is updated using a per-star sine function keyed to `Date.now()`, driving an independent twinkle cycle.

## Controls

| Key | Action |
|-----|--------|
| Left/Right Arrow | Rotate ship |
| Up Arrow / Tab | Thrust |
| Space | Fire |
| R | Restart (after game over) |
