# Architectural Wireframes

## Overview
This project presents an interactive monochrome design exploration interface, showcasing architectural wireframe models built with Three.js. The application displays three distinct house styles (Modern Split-Level, Victorian Townhouse, and Contemporary Multi-Level) rendered strictly through procedural wireframe geometries, offering an elegant abstract aesthetic.

## Core Mechanisms

### Three.js Scene Configuration
The core of the project relies on a customised Three.js scene featuring ambient and directional lighting to ensure the white wireframe elements stand out clearly against the dark background. The camera employs a spherical rotation system that automatically orbits the selected structure, which users can pause or reset.

### Procedural Geometry Construction
Instead of loading external 3D assets, each house is procedurally generated using primitive geometries such as boxes and custom buffer geometries for complex shapes like roofs and skylights. `THREE.EdgesGeometry` and `THREE.LineSegments` are used to extract and render only the structural edges of the forms:

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

### Dynamic State Management
The user interface allows seamless toggling between the three architectural designs via a bespoke control panel. Upon selection, the visibility array is updated instantly, and CSS animations provide visual feedback on the active model choice, creating a robust, fluid browsing experience.
