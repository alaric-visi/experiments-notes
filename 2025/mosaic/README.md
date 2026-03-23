# Mosaic Generator

## Overview
The Mosaic project is a browser-based generative art tool that produces highly customisable, animated grid patterns on an HTML5 Canvas. Users can manipulate variables such as grid size, mathematical pattern types, and colour palettes to procedurally generate shifting, tessellated visualisations.

## Core Mechanisms

### Mathematical Pattern Functions
The cornerstone of the application is the `getPatternValue` function. It takes grid coordinates (x, y) and an animated time variable, calculates the radial distance and angle from the canvas centre, and runs these parameters through various trigonometric equations depending on the selected pattern (e.g., Logarithmic Spiral, Radial Symmetry, Fibonacci Sequence).

```javascript
function getPatternValue(x, y, time = 0) {
    // ... calculates dx, dy, distance, and angle
    switch (pattern) {
        case 'spiral':
            return Math.sin(distance * 0.02 * complexity + angle * 3 + time * 0.05);
        case 'fibonacci':
            return Math.sin(distance * 0.01 * complexity + time * 0.03) *
                   Math.sin(angle * 1.618 + time * 0.02);
        // ... evaluates other mathematical patterns
        default:
            return Math.sin(distance * 0.01 * complexity + angle + time * 0.05);
    }
}
```

### Procedural Rendering and Normalisation
The output of the trigonometric functions continuously oscillates between -1 and 1. The `generateMosaic` loop normalises this float output to a range of [0, 1] to pick an appropriate colour mapping from the user's selected palette array.

```javascript
const value = getPatternValue(x, y, animationFrame);
const normalizedValue = (value + 1) / 2; // Converts [-1, 1] to [0, 1]

// Map the normalised value to an index within the discretised colour palette
const colorIndex = Math.floor(normalizedValue * colors.length);
const color = colors[Math.min(colorIndex, colors.length - 1)];

ctx.fillStyle = color;
ctx.fillRect( /* coordinates and dimensions */ );
```

### Throttled Responsiveness
To prevent the application from freezing when the user drags the configuration sliders, the application employs a `throttledUpdate` technique. It uses `setTimeout` to debounce the heavy canvas recalculation, executing the render only once the DOM input events have settled.
