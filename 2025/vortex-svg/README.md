# Vortex SVG

## Overview
This project utilises scalable vector graphics (SVG) and CSS animations to render a dynamic spiral vortex. It features automated colour theme shifting and an interactive click-event trail system.

## Core Mechanisms

### Mathematical Path Generation
The spiral paths are constructed programmatically in JavaScript. Trigonometry dictates the vertices along each vector path, creating expanding spiral rings around a central axis.

```javascript
for (let j = 0; j < 50; j++) {
    const progress = j / 50;
    const angle = progress * Math.PI * 6 + rotationOffset;
    const radius = progress * 500;
    const x = width / 2 + Math.cos(angle) * radius;
    const y = height / 2 + Math.sin(angle) * radius;
    points.push(`${j === 0 ? "M" : "L"} ${x} ${y}`);
}
```

### Colour Automation
A sequence of colour palettes transitions on a timed interval, updating the `stop-color` properties within the SVG's primary coordinate gradient (`radialGradient`), smoothly interpolating the hues.

### Interactive Particles
When the user clicks the document, an array of HTML elements is instantiated at the cursor's location, expanding outwards via the Web Animations API (`Element.animate()`) to simulate an explosive effect.
