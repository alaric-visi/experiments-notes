# Seven Sins

## Overview
This project is a static web page displaying an animated typographic sequence. It features a shifting gradient background, floating geometric particles, and three blurred orb elements. 

## Core Mechanisms

### Typographic Animation
The text elements are sequenced to fade in with cascading delays. Infinite CSS animations cycle the text colour and drop shadow properties between distinct hues (pink, cyan, and yellow).

### Particle System
JavaScript is used to dynamically instantiate particle elements. Each particle is assigned a random colour, horizontal drift, and variable duration, after which a timeout function removes it from the DOM.

```javascript
const duration = 15 + Math.random() * 10;
particle.style.animation = `floatParticle ${duration}s linear`;
// ...
setTimeout(() => particle.remove(), duration * 1000);
```

### Glowing Orbs
Three div elements are styled as large circles with a heavy `filter: blur(80px)` and infinite translation animations, creating ambient light spots across the gradient background.
