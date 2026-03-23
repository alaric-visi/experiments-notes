# Polytope Animator

## Overview
The Polytope Animator is an interactive web-based visualisation tool that renders rotating higher-dimensional geometric shapes (polytopes) onto a 2D HTML5 Canvas. It supports visualising shapes in dimensions ranging from 3D up to 6D, specifically including the hypercube (n-cube), simplex (n-simplex), and cross polytope (n-orthoplex).

## Core Mechanisms

### N-Dimensional Geometry Generation
The application mathematically constructs the vertices and edges for generic n-dimensional polytopes based on the user's selected dimension. For example, the `hypercube` function generates $2^n$ vertices and uses Hamming distance to determine which vertices share an edge.

```javascript
// Example: Hypercube generation logic snippet
function hypercube(n) {
    const V = 1 << n; const verts = new Array(V);
    for (let i = 0; i < V; i++) {
        const v = new Array(n);
        for (let b = 0; b < n; b++) v[b] = ((i >> b) & 1) ? 1 : -1;
        verts[i] = v;
    }
    // ... calculates edges by finding vertices with exactly 1 dimension of difference (Hamming distance of 1)
    return { verts, edges };
}
```

### High-Dimensional Rotation
To animate the rotation of an n-dimensional object, the application applies rotation matrices across all unique pairs of coordinate axes. The target angles are either automatically updated by the auto-spin functionality or manually modified via pointer drag events.

```javascript
// Rotating a vector across all axis pairs
for (let p = 0; p < pairs.length; p++) {
    const [i, j] = pairs[p];
    const a = currentAngles[p];
    const si = Math.sin(a), co = Math.cos(a);
    const xi = r[i], xj = r[j];
    
    // Apply 2D rotation to the mapped 2D plane within the n-dimensional space
    r[i] = xi * co - xj * si;
    r[j] = xi * si + xj * co;
}
```

### Perspective Projection
Rendering an n-dimensional object on a 2D screen requires projecting it down step-by-step. The `projectNDTo2D` function iteratively collapses the highest dimension into the remaining lower dimensions using a perspective divide, looping downwards until the coordinate reaches a standard 2D Cartesian pair (`x`, `y`) plus a `zDepth` value to inform line thickness and opacity.
