# Pocket Tiles

## Overview
This project is an interactive, physics-based grid simulation rendered on an HTML5 Canvas. The application visualizes multiple isolated "tiles" or cells within a grid containing dozens of uniquely colored sliding circles (referred to as stones). These elements bounce around endlessly, colliding with the boundaries of their parent cell and each other.

## Core Mechanisms

### Multi-Canvas Architecture
To optimize performance, the simulation does not draw every single stone on the main canvas during every frame. Instead, each "tile" (grid cell) is pre-rendered onto its own off-screen `<canvas>` element. When the main render loop fires, it simply draws these pre-rendered tile canvases onto the main screen grid.

```javascript
function makePhysicsTile(w, h, seed) {
    // Creates an off-screen canvas for the specific tile
    const c = document.createElement("canvas");
    c.width = Math.max(1, Math.floor(w));
    c.height = Math.max(1, Math.floor(h));
    const t = c.getContext("2d");

    // ... initializes stones and calculates localized physics ...

    return { canvas: c, step, render, stones };
}
```

### Physics and Collision Detection
The simulation implements a lightweight 2D physics engine. Each stone maintains properties for its position (`cx`, `cy`), velocity (`vx`, `vy`), radius, and base speed. In each animation frame, the `step(dt, time)` function is called to update velocities using trigonometric acceleration based on elapsed time, and to apply positional boundaries.

Circle-to-circle collisions are optimized using a spatial hash map to avoid checking every stone against every other stone (an O($n^2$) operation).

```javascript
// Example segment of collision resolution
const dxC = s2.cx - s1.cx;
const dyC = s2.cy - s1.cy;
const dist2 = dxC * dxC + dyC * dyC;
const minD = s1.r + s2.r;

if (dist2 > minD * minD || dist2 === 0) continue;

// Resolving overlap and applying elastic bounce based on restitution
const dist = Math.sqrt(dist2) || 0.0001;
const nx = dxC / dist;
const ny = dyC / dist;
// ... (momentum formulas utilizing the restitution constant)
```

### Responsive Grid Layout
The application dynamically calculates the optimal layout for the grid depending on the device's screen size (calculated in `chooseCounts()`). Variables adjust density, speed, and gap sizes when the user is on mobile (break point at 768px width) versus desktop to ensure a consistent frame rate.
