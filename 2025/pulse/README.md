# Capital & Subjectivity Terminal (Pulse)

## Overview
This project is an interactive, browser-based narrative terminal designed to slowly type out a speculative philosophical essay regarding capital, the subject, and the object. Designed with a retro command-line aesthetic, it controls the pacing of reading by animating text generation in batches and applying dynamic syntax highlighting to specific thematic keywords.

## Core Mechanisms

### Terminal Animation Engine
The core of the project is the `TerminalAnimator` JavaScript class. Instead of displaying all the text at once, it utilizes `setInterval` to iteratively type string batches into dynamically created DOM nodes.

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
    this.state.currentChar += batchSize;
    // ...
}
```

### Contextual Syntax Highlighting
As the text is generated, the `createStyledFragment` function checks each character's index against predefined criteria to assign distinct CSS classes. It searches the raw text for specific phrases defined in `isKeyConcept()` (e.g., 'structural misalignment') and `isCriticalTerm()` to apply `.highlight` and `.error` styles on the fly, mimicking code editors or analytical software.

### Accessibility and UX
The terminal includes several user experience enhancements:
- A custom blinking `.cursor` element appended to the active typing paragraph.
- Automatic scrolling (`handleAutoScroll()`) that keeps the latest text in view, which disables itself if the user manually intercepts the scroll wheel.
- An off-screen announcer `announceToScreenReader` function to politely alert screen readers of interface updates without breaking visual immersion.
