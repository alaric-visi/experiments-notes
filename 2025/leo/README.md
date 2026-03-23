# Leo Terminal

## Overview
This project presents an interactive narrative terminal interface, simulating a command-line environment that progressively types out a short sci-fi story about a character named Leo. The application utilises a custom JavaScript typing engine to handle text generation, programmatic specific styling, and dynamic formatting.

## Core Mechanisms

### Narrative Animation Engine
The central component of the project is the `TerminalAnimator` class, which manages the sequential typing of text. It uses request intervals to generate characters while handling DOM mutations efficiently. The text is processed in small batches to maintain a consistent visual typing speed across paragraphs.

```javascript
typeNextBatch() {
    // ...
    const remaining = para.length - this.state.currentChar;
    const batchSize = Math.min(this.batchSize, remaining);
    const textBatch = para.slice(this.state.currentChar, this.state.currentChar + batchSize);

    const fragment = this.createStyledFragment(
        this.state.currentPara,
        this.state.currentChar,
        textBatch
    );

    paraEl.insertBefore(fragment, this.cursorElement);
    // ...
}
```

### Contextual Text Styling
The source text is pre-processed to identify quoted dialogue and particular thematic keywords. This allows the application to apply distinct CSS classes dynamically during rendering, effectively differentiating between standard narrative text, highlights, and conceptual errors.

```javascript
isSpecialPhrase(paraIndex, charIndex) {
    const phrases = [
        'Leo',
        'white halls',
        'phonetic neutrality',
        'digital voice',
        'synthesised thread',
        'illegal data',
        'sensory record',
        'administrative machinery'
    ];
    // ...
}
```

### Accessibility and Utilities
The implementation includes features for screen readers using an `aria-live="polite"` region to broadcast status updates. It also binds native keyboard events to capture shortcuts, allowing users to safely copy the entire narrative text directly to their clipboard (`Ctrl+C`) or restart the animation cycle (`Space`).
