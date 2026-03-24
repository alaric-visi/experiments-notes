# After Truth, Comes Advertising

## Overview
This project presents a generative glitch art canvas animation rendering the phrase "AFTER TRUTH COMES ADVERTISING". It employs raw Pixel Manipulation via the HTML5 Canvas API to create real-time data moshing and bit-crushing effects.

## Core Mechanisms

### Glitch Sequencing
The animation operates on a four-phase cycle (calm, building, peak, and decay) determined by the elapsed time. This cycle dictates a global intensity multiplier applied to the visual distortions. Clicking on the canvas interactively boosts this intensity value.

### Pixel Sorting and Data Moshing
When the glitch intensity passes a threshold, the script reads the raw `ImageData` array of the canvas. It applies algorithms to sort pixels by brightness and overwrite sections of the buffer with displaced bytes, simulating video compression artifacts.

```javascript
for (let i = 0; i < glitchRows; i++) {
    const sourceRow = Math.floor(Math.random() * height) * width * 4;
    const targetRow = Math.floor(Math.random() * height) * width * 4;
    // Overwrite target with source pixels
}
```

### Chromatic Aberration
Text rendering is offset horizontally based on the glitch intensity. Three distinct fill passes (red, green, and blue) are drawn at slight positional offsets from the core white text to simulate RGB channel separation.
