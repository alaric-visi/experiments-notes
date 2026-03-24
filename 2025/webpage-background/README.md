# Fluid Background

## Overview
This project renders an animated, full-viewport background using an HTML5 Canvas. It is composed of four layered visual systems — blobs, wave lines, diagonal streaks, and stars — each managed by its own class and updated within a single `requestAnimationFrame` loop.

## Core Mechanisms

### GlassBlob
Each `GlassBlob` instance holds a position, velocity, rotation angle, pulse phase, and a fixed `hueOffset` for its colour. On each frame, it increments its `pulsePhase` and recalculates `size` using a sine function, producing a continuous breathing effect. The blob's shape is drawn as an eight-point polygon whose radial distance is perturbed by:

```javascript
const r = this.size * (0.8 + Math.sin(a * 3 + this.pulsePhase) * 0.2);
```

A radial gradient is applied using three `hsla` stops offset by 0, 80, and 160 degrees from `hueOffset`, shifting the hue across the radius of each blob. The canvas `filter` property is set to `blur(30px)` at draw time, softening the edges to create the glassmorphic look. Eight blobs are instantiated with random positions; their positions wrap around a 200px boundary margin so they never disappear abruptly.

### WaveLine
A single `WaveLine` instance is created at the vertical midpoint of the canvas. Its `draw` method iterates over 20 stacked sub-lines, each offset vertically by `this.spacing`. For each sub-line, `x` increments by 5 pixels across the canvas width, and the y-coordinate is computed using:

```javascript
const y = this.startY + Math.sin(x * this.frequency + this.offset) * this.amplitude + line * this.spacing;
```

`this.offset` advances by `this.speed` each frame, causing the wave to scroll horizontally. Every third sub-line additionally renders small filled circles at 80-pixel intervals along the wave path, acting as node markers.

### DiagonalStreak
Eight `DiagonalStreak` instances represent falling light streaks. Each has a random starting `x` position, a `length` between 100 and 250 pixels, and a `speed` between 2 and 5. On each update, `x` advances at half the speed whilst `y` advances at full speed, creating a shallow diagonal trajectory. The streak is drawn as a line with a linear gradient applied along its length, fading to transparent at both ends. When a streak exits the bottom of the canvas, its `reset()` method repositions it at the top.

### Star
Eighty `Star` instances each have a fixed random position and a `twinkleSpeed` that oscillates `opacity` between 0 and 1 by reversing sign when either boundary is reached. They are drawn as small filled arcs.

### Corner Grid
`drawCornerGrids()` renders two fixed 2×8 dot grids — one in the top-right corner and one in the bottom-left — using hardcoded offsets from the canvas edges. These are redrawn on every frame as part of the main `animate` loop.

### Render Loop
The `animate` function begins by filling the entire canvas with a four-stop diagonal linear gradient ranging from `#0f0a1f` to `#0a1628`, establishing the deep purple-navy base. It then calls `update()` and `draw()` on each entity in a fixed order: stars, streaks, blobs, wave lines, and finally the corner grids. `requestAnimationFrame` schedules the next call.

A `resize` event listener on `window` updates `canvas.width` and `canvas.height` to match the viewport; this clears the canvas and resets dimensions, but the entity positions are not recalculated on resize, so blobs and stars may temporarily cluster before drifting.
