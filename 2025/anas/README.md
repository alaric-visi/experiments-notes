# Anas

## Overview
This project presents an animated, wireframe pterosaur model built entirely through code using Three.js. The model executes continuous flight animations alongside ambient lighting.

## Core Mechanisms

### Model Assembly
The creature is constructed from multiple independent primitive geometries (including spheres, cylinders, cones, and custom shapes for the wing membranes). The components are merged into a unified `THREE.Group()` to function as a singular entity.

### Wing Membrane Logic
The wing spans are mapped mathematically using `THREE.Shape()` to define their vertices. A shape geometry is then generated from this custom path and rendered using a transparent, double-sided material.

```javascript
const leftMembraneShape = new THREE.Shape();
leftMembraneShape.moveTo(0, 0);
leftMembraneShape.lineTo(-3, 1);
leftMembraneShape.lineTo(-3.5, -1);
leftMembraneShape.lineTo(0, -0.5);
```

### Animation Loop
Within the `requestAnimationFrame` loop, trigonometric functions adjust the rotation and vertical position of the group to simulate hovering and motion dynamics.
