# Architectural Wireframes

## Overview
This project presents an interactive interface for viewing architectural wireframe models built with Three.js. The application displays three distinct house styles (Modern Split-Level, Victorian Townhouse, and Contemporary Multi-Level) rendered through procedural wireframe geometries.

## Core Mechanisms

### Three.js Scene Configuration
The project is built around a Three.js scene featuring ambient and directional lighting to ensure the white wireframe elements stand out against the dark background. The camera employs a spherical rotation system that automatically orbits the selected structure, which users can pause or reset.

### Procedural Geometry Construction
Instead of loading external 3D assets, each house is generated using primitive geometries such as boxes, as well as custom buffer geometries for shapes like roofs and skylights. `THREE.EdgesGeometry` and `THREE.LineSegments` are used to extract and render the structural edges:

```javascript
function createEdges(geometry, color = 0xffffff) {
    const edges = new THREE.EdgesGeometry(geometry);
    const line = new THREE.LineSegments(
        edges,
        new THREE.LineBasicMaterial({
            color: color,
            linewidth: 1.5,
            transparent: true,
            opacity: 0.9
        })
    );
    return line;
}
```

### State Management
The user interface provides a control panel for switching between the three architectural designs. Upon selection, the visibility array is updated instantly to display the active model choice.
