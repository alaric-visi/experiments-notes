# Crystal Cave

## Overview
This project presents an interactive, First-Person 3D cave environment constructed directly within the browser using React and the Three.js library. The application leverages custom geometry manipulations and complex material rendering characteristics to visualize procedural terrain, luminous crystalline formations, and dynamic lighting.

## Core Mechanisms

### Procedural Geometry Deformation
To construct an irregular, natural-looking environment without standard 3D modelling tools, the application applies positional mathematical noise to base primitives before resolving their structures. The main floor and ceiling planes map arrayed height offsets across their vertices, whilst the `createRock` function takes standard `SphereGeometry` items and displaces each node spatially (`rockVertices[i] *= (1 + noise)`) to simulate the unpolished and jagged contours of natural stone.

### Crystalline Logic and Materials
The scene's glowing crystals are formed by assembling multiple basic `ConeGeometry` meshes into a grouped lattice. The engine leverages `MeshPhysicalMaterial` for high-fidelity light rendering, configuring extensive transparency logic such as `clearcoat`, `roughness`, and `emissive` outputs. These mesh definitions are paired with arrayed ambient and highly saturated `PointLight` sources to execute internal refraction simulations throughout the digital cave.

### Native User Navigation
User interaction implements a native First-Person movement mechanism bound via standard event listener bindings. Drag and rotation events track delta `movementX` measurements translating into horizontal view pivoting (`camera.rotation.y`), whilst keyboard combinations (W/A/S/D) intercept axis codes within the overarching `requestAnimationFrame` loop, translating the camera forward or sideways (`camera.translateZ`) relative to the current perspective normal.
