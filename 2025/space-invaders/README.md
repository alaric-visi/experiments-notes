# Space Invaders

## Overview
Space Invaders is a robust HTML5 Canvas arcade game modeled directly after classic Space Invaders. It successfully implements the traditional mechanics of horizontal player movement, bullet collisions, and descending enemy formations, but distinguishes itself with an escalating, algorithmic "glitch" aesthetic that corrupts the visual output during gameplay.

## Core Mechanisms

### The Game Loop and Entity Management
The game runs on a standard `requestAnimationFrame` loop split into `update()` (physics, input handling, collision detection) and `render()` (canvas drawing). Entities like the player, enemies, bullets, and particles are maintained in modular arrays or objects, allowing for straightforward O(n * m) bounding-box collision checks.

```javascript
bullets.forEach((bullet, bIndex) => {
    enemies.forEach((enemy, eIndex) => {
        // AABB (Axis-Aligned Bounding Box) Collision Detection
        if (enemy.alive &&
            bullet.x < enemy.x + enemy.width &&
            bullet.x + bullet.width > enemy.x &&
            bullet.y < enemy.y + enemy.height &&
            bullet.y + bullet.height > enemy.y) {

            enemy.alive = false;
            bullets.splice(bIndex, 1);
            score += (enemy.type === 0) ? 30 : (enemy.type === 1) ? 20 : 10;
            // ... spawn explosion particles
        }
    });
});
```

### Algorithmic Glitch Aesthetics
The standout feature is the `applyGlitch()` function, which runs during the render phase to simulate graphical corruption and "reality breaks." This is achieved through a mix of advanced Canvas API compositing features and direct memory manipulation of pixel data.

```javascript
// Direct manipulation of the Canvas pixel array
const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
const data = imageData.data;

for (let i = 0; i < data.length; i += 4) {
    if (Math.random() < 0.05) {
        const corruption = Math.random();
        // Shift colors completely
        if (corruption < 0.66) {
            data[i] = Math.random() * 255;
            data[i + 1] = Math.random() * 255;
            data[i + 2] = Math.random() * 255;
        } else {
            // Invert colors
            data[i] = 255 - data[i];
            data[i + 1] = 255 - data[i + 1];
            data[i + 2] = 255 - data[i + 2];
        }
    }
}
ctx.putImageData(imageData, 0, 0);
```

### Mobile Input Handling
The game features responsive scaling (`resizeCanvas()`) using CSS transforms and dynamically positioned touch controls. Built-in standard `touchstart` and `touchend` event listeners update a master `keys` dictionary, meaning the core `update()` loop handles mobile button presses identically to physical keyboard inputs without requiring duplicated logic.
