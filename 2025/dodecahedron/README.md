# Dodecahedron

## Overview
This project renders a continuously rotating 3D dodecahedron exclusively using the 2D Canvas API. It computes the projection mathematics manually without relying on WebGL or external 3D libraries.

## Core Mechanisms

### 3D to 2D Projection
The shape's geometry is defined by an array of 20 3D vertices based on the golden ratio (`phi`). Each frame, rotation matrices are applied to the `X`, `Y`, and `Z` coordinates before projecting them onto the 2D plane through uniform division by depth.

```javascript
function project(point) {
    const scale = perspective / (perspective + point[2] * size);
    return [
        center.x + point[0] * size * scale,
        center.y + point[1] * size * scale,
        point[2]
    ];
}
```

### Depth Sorting (Painter's Algorithm)
To ensure the correct visual overlap of the solid faces, the algorithm calculates the average `Z-depth` of each face's projected vertices. The array of faces is then sorted (`a.avgZ - b.avgZ`) so that the furthest polygons are drawn first.

### Dynamic Colouration
The fill colour of each polygon transitions smoothly using the HSL colour space, with the hue bound to the elapsed animation time and the face's index. Brightness is modulated according to the face's Z-depth to simulate ambient occlusion.
