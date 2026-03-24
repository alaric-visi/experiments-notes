# Constants

## Overview
This project renders an animated, full-viewport canvas visualisation built around the golden ratio (φ ≈ 1.618). Two distinct geometric systems — a logarithmic φ-spiral and a set of recursively nested vortex arms — are composited each frame using partial canvas clearing to produce a persistent-trail effect.

## Core Mechanisms

### The Golden Ratio
φ is defined once and used throughout:

```javascript
const PHI = (1 + Math.sqrt(5)) / 2;
```

It governs spiral growth rates, nested vortex radii, vortex rotation speeds, and the radii of the concentric ring system, ensuring geometric proportionality across all drawn elements.

### Partial Clear and Trail Effect
The `animate` function does not clear the canvas with a fully opaque fill. Instead, it paints a semi-transparent rectangle each frame:

```javascript
ctx.fillStyle = 'rgba(5, 5, 15, 0.15)';
ctx.fillRect(0, 0, canvas.width, canvas.height);
```

An opacity of 0.15 means previous frames fade out gradually over approximately six to seven subsequent frames, producing motion trails on all moving elements without storing any prior draw state.

### φ-Spiral (`drawPhiSpiral`)
`drawPhiSpiral` plots a logarithmic spiral using the recurrence `r = R_max × φ^(θ / 2π − 3)`, stepping `θ` from 0 to `6π` in increments of 0.1 radians. This gives three full turns, with the radius growing by a factor of φ on each complete revolution. Two spirals are drawn per frame — one clockwise in blue, one counter-clockwise in orange — both rendered at 20% opacity so they serve as a structural underlay.

### Vortex Arms (`drawVortex`)
`drawVortex(x, y, radius, rotation, color, isInward, depth)` draws eight spiral arms radiating from a given point. For each arm, `r` steps from 0 to `radius` in increments of 2 pixels. At each step, the angular position of the point incorporates a φ-based winding rate:

```javascript
const spiralAngle = angle + (isInward ? -r : r) / (radius / (PHI * 3));
```

For inward vortices, the radius is also contracted slightly as it grows: `dist = r × (1 − r / radius × 0.3)`, pulling the outer arm tips inward. Each arm is stroked using a radial gradient that fades from opaque at the centre to near-transparent at the edge, with colour and opacity scaled by recursion depth.

Recursion is capped at `maxDepth = 1`. When `depth < maxDepth`, five child vortices are spawned at positions arranged in a regular pentagon at distance `radius / (φ × 1.5)`, each with radius `radius / φ`. Child vortices invert the `isInward` flag, so inward vortices spawn outward children and vice versa.

### Vortex Pair Arrangement
Six pairs of vortices are placed symmetrically around the canvas centre. For pair `i`, the base angle is:

```javascript
const pairAngle = (i * Math.PI * 2) / 6 + time * 0.2;
```

The blue (inward) vortex sits at angular position `pairAngle` and distance 25% of the minimum canvas dimension from centre. Its rotation advances at `time × (1/φ)`. The orange (outward) vortex sits at the diametrically opposite position (`pairAngle + π`) and rotates at `time × φ`. The use of `1/φ` and `φ` as rotation multipliers for opposing vortices means their angular velocities maintain a φ ratio throughout the animation.

### Concentric Rings
Twelve concentric rings are stroked at 10% white opacity. Each ring's radius is calculated as:

```javascript
const radius = 150 + Math.sin(time + i) * 30;
ctx.arc(centerX, centerY, radius * Math.pow(PHI, i % 3 - 1), 0, Math.PI * 2);
```

The `Math.pow(PHI, i % 3 - 1)` term cycles through multipliers of `1/φ`, `1`, and `φ` across every three rings, creating three radial tiers. The `sin(time + i)` offset causes each ring to pulse in and out at a slightly different phase.

### Animation Control
A `visibilitychange` listener cancels the `requestAnimationFrame` loop when the page is hidden and restarts it via `startAnimation()` on visibility restoration, preventing unnecessary GPU work whilst the tab is inactive.
