# EchoLogix

## Overview
This project explores interactive digital narrative through multiple interfaces. It includes a simulated command-line interface progressively typing out a short sci-fi story (`fragments-of-finn.html`), a dedicated layout containing the wider EchoLogix lore (`stories.html`), and a custom interactive media player featuring AI avatars narrating the text (`videos.html`).

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

### Narrative Navigation & Animation
The `stories.html` page implements a responsive layout with a slide-out sidebar for mobile devices. It utilises the GSAP animation library, specifically the Flip plugin, to smoothly transition between different story sections without page reloads, adjusting opacity and vertical translation dynamically.

```javascript
Flip.from(state, {
    duration: 0.6,
    ease: "power1.inOut",
    opacity: true,
    scale: true,
    onComplete() {
        gsap.timeline({ defaults: { ease: "power2.out" } })
            .to(incoming.querySelector("h2"), { opacity: 1, y: 0, duration: 0.45 })
            // ... sequence for fading in paragraphs
    }
});
```

### Custom HTML5 Video Player
The `videos.html` file features a fully bespoke media player interface overlaying a standard `<video>` element. Features include custom volume sliders, timeline scrubbing, fullscreen toggling, and programmatic switching between the 9 different story parts via data attributes. It includes its own custom CSS styling and floating interaction controls.

```javascript
function loadVideo(index) {
    currentVideoIndex = index;
    videoPlayer.src = videos[index];
    currentVideoText.textContent = videoTitles[index];
    
    videoPlayer.load();
    videoPlayer.addEventListener('loadeddata', function playAfterLoad() {
        videoPlayer.play().then(() => {
            isPlaying = true;
            updatePlayPauseButton();
        });
    }, { once: true });
}
```
