# Geometries

## Overview
This project is a collection of seven interactive mathematical geometry visualisations, rendered directly onto HTML5 string-based `<canvas>`. The experiments focus on projecting high-dimensional objects (such as 4D tesseracts and glomes) into 2D space, alongside parametric surface generation. All files include an overlay control panel for manipulating rotation, geometry density, and rendering parameters in real-time.

## Project Files

### 1. Tesseract 1 (`tesseract-1.html`)
Renders a 4-dimensional hypercube projected into 3D and subsequently into 2D space. The edges of the tesseract are drawn using an intricate, Escher-inspired procedural pattern. The projection mathematics applies a perspective division based on the `w` depth coordinate before executing the standard `z` depth division.

```javascript
function project4Dto3D(p) {
    const distance = 4;
    const w = 1 / (distance - p.w);
    return {
        x: p.x * w,
        y: p.y * w,
        z: p.z * w
    };
}
```

### 2. Tesseract 2 (`tesseract-2.html`)
An alternative rendering of the 4D hypercube that draws standard, solid lines between the 16 vertices. It focuses on a clean, neon aesthetic where edges smoothly pulse in thickness according to a sine wave function based on current time coordinates.

### 3. Glome 1 (`glome-1.html`)
Visualises a 4-dimensional hypersphere (a glome). The geometry is generated using parametric angles (`phi1` and `phi2`) across a defined detail level. The rendering structure acts against a background grid of hexagons that react with expanding "burst" animations when points from the glome intersect them.

```javascript
const x = radius * Math.cos(phi1) * Math.sin(phi2);
const y = radius * Math.sin(phi1) * Math.sin(phi2);
const z = radius * Math.cos(phi2);
const w = radius * Math.sin(phi1 + phi2) * 0.5;
```

### 4. Glome 2 (`glome-2.html`)
A variation of the hypersphere visualisation that places the object on top of a procedural background grid constructed from 9-sided nonagons. Points close to one another in the projection space are dynamically connected with contextual lines to form a wireframe mesh.

### 5. Klein Bottle (`klein-bottle.html`)
Generates a 3-dimensional non-orientable surface (the mathematical Klein Bottle) and runs it through the same 4D rotation pipelines as the other experiments. The rendering logic maps parametric `u` and `v` coordinates to a continuous self-intersecting shape, drawing connection intervals between adjacent vertices in the point cloud.

### 6. Quartic Plane Curve (`quartic-plane-curve.html`)
A 2D mathematical curve generator that can transition between five distinct curve architectures, including bounding spirals, butterfly equations, and clover curves. It employs an interactive control panel permitting the user to switch the active equation dynamically on the underlying canvas API.

### 7. Lemniscate of Bernoulli (`lemniscate-of-bernoulli.html`)
Specifically visualises the lemniscate curve (resembling the infinity symbol). The code generates three independent layers of the mathematical curve, applying phase offsets to their individual geometric scale and matrix rotation to create a continuous, intertwined animation effect.

```javascript
const sinT = Math.sin(animatedParam);
const cosT = Math.cos(animatedParam);
const denominator = 1 + sinT * sinT;

const x = (a * cosT) / denominator;
const y = (a * sinT * cosT) / denominator;
```
