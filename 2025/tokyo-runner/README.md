# Tokyo Runner

## Overview
This project is a text-based cyberpunk adventure game set in Neo-Tokyo. It presents a branching narrative where the user navigates through various scenarios, managing resources such as time and credits to achieve multiple distinct endings. 

## Core Mechanisms

### State Management
The game's progression is governed by a centralised state containing variables for the current scene, player credits, and remaining time. These parameters are updated dynamically during scene transitions via the `updateStatus()` function, seamlessly reflecting the consequences of the user's choices.

### Branching Narrative Engine
The story is structured within a comprehensive `scenes` object. Each node in this object represents a distinct state defining the descriptive `text`, an array of `choices` linking to subsequent nodes (`nextScene`), and the associated resource modifiers (`timeCost`, `creditChange`).

```javascript
investigate: {
    id: "investigate",
    text: "You boot up your hacking rig...",
    choices: [
        { text: "Accept the job - you need the money", nextScene: "alley" },
        // ... additional choices
    ],
    timeCost: 1,
    creditChange: 0
}
```

### Dynamic Text Rendering
To enhance the cyberpunk aesthetic, a procedural typing effect is applied to the narrative output. This is achieved using `requestAnimationFrame`, which sequentially renders the text content while temporarily disabling user interaction to simulate a real-time terminal interface.
