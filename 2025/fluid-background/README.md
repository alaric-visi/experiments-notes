# Fluid Background

## Overview
This project renders a continuous, procedurally generated ambient background within an HTML5 Canvas. The scene combines multiple animated components to construct a deep and dynamic environment, comprising glass-like blobs, intersecting wave structures, moving streaks, and twinkling stars.

## Core Mechanisms

### Glass Blobs
Instances of the `GlassBlob` class simulate floating, organic orbs of light. These are drawn using radial gradients with distinct transparency steps and an overarching CSS `blur` filter (`ctx.filter = 'blur(30px)'`) applied to the rendering context. Their size oscillates smoothly over time via trigonometric functions (`Math.sin(this.pulsePhase)`), providing a natural breathing aesthetic.

### Wave Structures
The `WaveLine` class creates intersecting horizontal patterns by layering multiple stroked paths on top of each other. The paths calculate their independent Y-coordinates using sine waves (`Math.sin(x * this.frequency + this.offset) * this.amplitude`), leading to an overlapping undulating effect modified by fixed speed constants.

### Environmental Depth
Additional depth is introduced through instances of `DiagonalStreak` which form fast-moving linear gradients acting as light streaks, alongside a background field of `Star` entities governed by their own oscillating opacity logic. Finally, a `drawCornerGrids()` routine maps geometric dot matrices in the corners to anchor structure against the free-flowing organic background elements.
