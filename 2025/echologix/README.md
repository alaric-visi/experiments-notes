# EchoLogix: Fragments of Finn

## Overview
This project is an interactive, browser-based narrative terminal. It visually simulates a command-line interface progressively typing out a short sci-fi story. The application features auto-scrolling, programmatic text styling, and clipboard integration for copying the narrative.

## Core Mechanisms

### Terminal Animation Engine
The core logic resides within the `TerminalAnimator` class. This class handles the state of the typing animation, tracking the current paragraph and character index. It uses `setInterval` to output characters in configurable batch sizes to simulate variable typing speed.

```javascript
typeNextBatch() {
    // ... animation pause logic ...
    const remaining = para.length - this.state.currentChar;
    const batchSize = Math.min(this.batchSize, remaining);
    const textBatch = para.slice(this.state.currentChar, this.state.currentChar + batchSize);

    const fragment = this.createStyledFragment(
        this.state.currentPara,
        this.state.currentChar,
        textBatch
    );

    paraEl.insertBefore(fragment, this.cursorElement);
    this.state.currentChar += batchSize;
    // ... auto scroll handling ...
}
```

### Text Processing & Syntax Highlighting
Before typing starts, `preprocessText()` splits the raw text into paragraphs and pre-computes the indices of quotation marks. When `createStyledFragment` builds DOM nodes, it references these regions to apply syntax-highlighted CSS classes (`highlight`, `error`, `emphasis`) to specific dialogue strings dynamically.

```javascript
const inQuote = this.isInQuotedText(paraIndex, globalIdx);
let charClass = '';
if (inQuote) {
    if (this.isSpecialPhrase(paraIndex, globalIdx)) {
        charClass = 'highlight';
    } else if (this.isErrorPhrase(paraIndex, globalIdx)) {
        charClass = 'error';
    } else {
        charClass = 'emphasis';
    }
}
```

### Accessibility and Utilities
The class binds native browser events to detect scrolling (to properly disable auto-scrolling without fighting the user) and captures keyboard events (like `Ctrl+C` to copy the underlying text or `Space` to restart). It also pushes status updates to a hidden `aria-live` polite region for screen readers.
