# Julia Set Visualizer

## Overview
This project is an interactive, browser-based fractal generator that visualizes the Julia Set. It uses an HTML5 Canvas and JavaScript to compute and render the fractal in real-time. The application provides user controls to adjust the depth of iterations and select different color mapping palettes.

## Core Mechanisms

### Canvas and State Management
The state of the fractal renderer is maintained in a central object. This tracks viewport parameters (center, scale), fractal parameters (complex constant `c`, max iterations), and rendering settings.

```javascript
const state = {
    center_x: 0,
    center_y: 0,
    scale: 1.0,
    max_iter: 10,
    palette: 0,
    c_re: -0.7,
    c_im: 0.27015,
    frame: 0,
    width: 0,
    height: 0,
    animationId: null
};
```

### Fractal Computation
The core mathematical evaluation happens per-pixel within the `renderFractal()` function. The algorithm determines how quickly complex numbers in the current viewpoint escape the bound of radius 2.

```javascript
let i = 0;
while (zx * zx + zy * zy <= 4 && i < state.max_iter) {
    const tmp = zx * zx - zy * zy + cx0;
    zy = 2 * zx * zy + cy0;
    zx = tmp;
    i++;
}
```
If `i` reaches `state.max_iter`, the point belongs to the set and is painted black. Otherwise, its escape velocity `t = i / state.max_iter` determines its color.

### Color Palettes
Colors are determined using procedural functions mapped to the normalized iteration count `t` (from 0.0 to 1.0). For example, the "Ocean" palette interpolates mostly blue and green properties based on `t`.

```javascript
t => {
    const r = Math.floor(20 + 50 * t);
    const g = Math.floor(100 + 100 * t);
    const b = Math.floor(200 + 55 * t);
    return [r, g, b];
}
```

### Animation
The visualization includes a continuous animation loop where the complex constant `c_re` and `c_im` are dynamically modulated using trigonometric functions based on the current frame time, shifting the layout of the fractal constantly.

```javascript
function animate() {
    state.frame++;
    const t = state.frame * 0.01;
    state.c_re = 0.7885 * Math.cos(t);
    state.c_im = 0.7885 * Math.sin(t);
    renderFractal();
    state.animationId = requestAnimationFrame(animate);
}
```
