# Strudel Loop

## Overview
This JavaScript file contains live-coding pattern logic written for **Strudel**, the JavaScript port of the popular algorithmic live-coding music platform, TidalCycles. Set to a slow, funky tempo (25/4 CPM), it generates a full, dynamic musical arrangement featuring algorithmic drum beats, heavily filtered synthesisers, and modal basslines.

## Core Mechanisms

### Procedural Generation
Instead of hardcoding every measure of the 16-bar verses, the script uses pure JavaScript logic to generate continuously evolving permutations. Functions like `genKick` mutate an array of predefined 16th-note drum grids using deterministic boundaries and stochastic probabilities. 

```javascript
const genKick = (i) => {
  let grid = seedKickGrids[i % seedKickGrids.length].slice() 
  for (let s = 0; s < grid.length; s++){
    // Probabilistically add skip notes or fill empty gaps
    if (grid[s] === "bd" && rand() < 0.15) grid[s] = "~"
    if (grid[s] === "~"  && rand() < 0.08) grid[s] = "bd"
  }
  return grid.join(' ')
}
```

### Arrangement and Pattern Stacking
The patterns are combined into complete audio sequences using native Strudel composition methods. Drum grids are parsed by the `s()` pattern evaluator, layered contextually using `stack()`, and eventually concatenated sequentially to create entire sections (like intros, verses, and choruses).

```javascript
let vKickBars  = Array.from({length:16}, (_,i)=> s(genKick(i)).bank('crate').gain(1.05))
let vSnareBars = Array.from({length:16}, (_,i)=> s(genSnare(i)).bank('crate').gain(0.95).late(0.012))

// Combining Kick, Snare, and HiHat arrays
let vDrumBars  = vKickBars.map((_,i)=> stack(vKickBars[i], vSnareBars[i], vHatBars[i]))
let verseDrums = cat(vDrumBars)
```

### Modal and Tonal Manipulation 
Beyond percussive randomness, the script implements melodic shifts using strict arrays of notes in specific scales (e.g., `['e2','g2','a2','b1','d2','e2','g2','a2']`). Furthermore, standard audio effects like Low-Pass Filters (`.lpf`), Bitcrushing (`.crush`), and specific micro-timing swing parameters (`swingMask`) are all programmatically encoded into the Strudel-safe audio wrappers.
