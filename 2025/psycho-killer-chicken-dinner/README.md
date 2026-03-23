# Psycho Killer, Chicken Dinner

## Overview
This visual experiment models a continuous semantic clustering and vector embedding simulation using the HTML5 Canvas API. By representing arbitrary strings of text (parsed from "PSYCHO KILLER CHICKEN DINNER") as high-dimensional data points, the simulation cycles through sequential phases that mimic the underlying mechanisms of modern Large Language Model (LLM) vector databases. 

## Core Mechanisms

### Simulated Vector Embeddings
When the text is tokenised, each word instantiates a `Vector` object containing a 16-dimensional array of pseudo-random floating-point values generated using trigonometric hashing based on the word's characters and length. 

```javascript
const dimensions = 16;
const values = Array.from({ length: dimensions }, (_, i) => {
    // Generate deterministic pseudo-random values simulating word embeddings
    const base = Math.sin(word.charCodeAt(0) + i) * 0.8;
    const semantic = Math.cos(word.length + i * 0.7) * 0.6;
    const random = (Math.random() - 0.5) * 0.4;
    return base + semantic + random;
});
```

### Phase Orchestration
The simulation continuously cycles through visually distinct phases, managed primarily within the `update` function and `drawPhase` switch statements:
1. **Vectorisation:** Calculates and visualises the cosine similarity between adjacent multi-dimensional vectors using dot products.
2. **Embedding Space:** Maps the abstract 16-dimensional points onto a 2D Cartesian plane using fluid trigonometric wave functions.
3. **Semantic Clustering:** Groups nodes into thematic clusters, pulling them gravitationally towards hardcoded coordinate centres representing thematic boundaries.
4. **Vector Transformation:** Applies a morph factor across the data arrays to represent dimensional translation. 

### Cosine Similarity Visualisation
During the Vectorisation phase, the relationship between words is determined mathematically via normalising their dot products against their magnitudes. This continuous relationship then dictates the line widths, dashboards, and opacities separating the floating vector nodes.

```javascript
let dotProduct = 0;
let mag1 = 0;
let mag2 = 0;

for (let j = 0; j < v1.values.length; j++) {
    dotProduct += v1.values[j] * v2.values[j];
    mag1 += v1.values[j] * v1.values[j];
    mag2 += v2.values[j] * v2.values[j];
}

const similarity = dotProduct / (Math.sqrt(mag1) * Math.sqrt(mag2));
// Similarity dictates visual rendering weight
```
