# Three.js Experiments

## Overview
This project acts as a container for multiple procedural Three.js wireframe models. The application provides an interface to toggle between four distinct scenes: a bee, a skull, a subterranean cave structure, and a xenomorph model. 

## Core Mechanisms

### Scene Switching Logic
The application manages memory by clearing existing objects from the scene before instantiating the new requested model. Buttons map to discrete load functions for each entity.

```javascript
if (scene) {
    while (scene.children.length > 0) {
        scene.remove(scene.children[0]);
    }
}
// Load requested model based on parameters
```

### Procedural Generation
Rather than using external `.gltf` or `.obj` files, all models are constructed programmatically using native Three.js primitives (`SphereGeometry`, `CylinderGeometry`, `ShapeGeometry`, etc.).

### Dual-Canvas Architecture
The background relies on a traditional 2D Canvas context rendering a pixelated gradient algorithm, whilst the primary Three.js WebGL rendering layer sits transparently on top to present the 3D structures.
