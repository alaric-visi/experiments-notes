# Hypersphere

## Overview
This project visualises a 4-dimensional hypersphere (or glome) projected onto a 2D HTML5 Canvas. The simulation uses native 2D rendering and manual matrix calculations to compute the high-dimensional rotations, drawing thousands of connected nodes and trailing comet effects.

## Core Mechanisms

### Mathematical 4D to 2D Projection
The hypersphere's geometry begins as random 4D vectors on the unit hypersphere surface using Gaussian distributions. In each animation frame, these vectors undergo a sequence of 4D rotations (affecting the `XY`, `ZW`, `XZ`, `YW`, `XW`, and `YZ` planes). The resulting `(X, Y, Z, W)` coordinates are projected first into 3D using a perspective division by `W`, and then into 2D display space through a second perspective division by `Z`.

```javascript
const denom4 = (wCam - w);
const s4 = f4 / (denom4 <= 0.05 ? 0.05 : denom4);
let x3 = x * s4, y3 = y * s4, z3 = z * s4;

const denom3 = (zCam - z3);
const s3 = f3 / (denom3 <= 0.05 ? 0.05 : denom3);
```

### Rendering Architecture
The visual aesthetic is achieved using context global composite operations (`lighter`) for additive blending. It draws static background stars, individual hypersphere nodes connected by proximity lines, and high-opacity trails for designated "comet" nodes. 

### Interactive Tilting
Event listeners track pointer movement across the viewport, storing normalized `X` and `Y` coordinates to apply a continuous tilt offset to the ongoing 4D rotation axes, ensuring responsive interaction from the user.
