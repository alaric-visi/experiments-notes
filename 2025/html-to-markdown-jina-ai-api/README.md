# HTML to Markdown — Jina AI API

A single-page tool that invokes the Jina AI Reader API (`r.jina.ai`) to fetch a remote URL and return its content as Markdown. An animated wave background is rendered on a fixed Canvas element behind the main card interface.

## Overview

The page is structured into two glassmorphic cards: one for URL input and one for output. A separate `<textarea id="tempTextarea">` is positioned off-screen at `left: -9999px` for use by the clipboard fallback path.

## Core Mechanisms

### Wave Background

Four `Wave` instances are instantiated at staggered y-positions (60%, 65%, 75%, and 85% of canvas height) with distinct amplitude, frequency, speed, and colour parameters. Each wave is drawn by iterating x-coordinates at a step of 5 px (10 px on viewports narrower than 768 px) and computing:

```
waveY = this.y + sin(x × frequency + time × speed + offset) × amplitude
```

The path is closed back to `canvas.height` and filled with a radial gradient centred on `(canvas.width / 2, this.y)`, fading the wave colour from full opacity at centre to 30% opacity at the canvas edge. A linear background gradient runs diagonally from steel blue through electric blue, red, and orange. The animation loop is throttled to 30 fps using a comparison of `currentTime - lastTime` against a 33 ms interval constant.

### API Request

On submission, `convertToMarkdown` constructs the Jina Reader URL as:

```
https://r.jina.ai/{encodeURIComponent(url)}
```

The fetch is dispatched with three headers: `Authorization: Bearer {API_KEY}`, `X-Return-Format: markdown`, and `Accept: text/plain`. The function first validates the input with `new URL(url)` to catch malformed strings before making the network call. The fetch function is resolved via `window.proxyFetch || window.fetch`, allowing a proxy layer to be injected without modifying the core logic.

### Loading State

`setLoading` disables the convert button and replaces its text content with an inline spinner element (`<span class="spinner">`) and the label "Converting…". The spinner is a 1.25 rem circle with a `border-top-color` of `#3b82f6` rotating at `0.8s linear infinite`. The `prefers-reduced-motion` media query sets `animation-duration: 0.01ms` globally and removes the rotation from the spinner, replacing it with a static broken-ring appearance.

### Output Rendering

`showMarkdown` assigns the raw response text to `outputDiv.textContent` rather than `innerHTML`, preventing any HTML in the Markdown string from being interpreted by the browser. The output container uses `white-space: pre-wrap` and `overflow-wrap: break-word` to preserve line breaks while preventing content from overflowing the card boundary. A `max-height: 500px` with `overflow-y: auto` constrains the display area.

### Clipboard

`copyToClipboard` attempts `navigator.clipboard.writeText`. On failure or in environments without Clipboard API support, `fallbackCopyText` writes the content to `tempTextarea`, calls `select()` and `setSelectionRange(0, 99999)`, then invokes `document.execCommand('copy')`. In both paths, success triggers `showCopySuccess`, which temporarily changes the button label to "Copied!" and adds a `.copied` class that plays a 0.5 s scale-down keyframe animation.

### Responsive Layout

Below 640 px, the input group stacks vertically (the URL field above, the button below) and both elements expand to full column width with a minimum touch target height of 44 px. At 640 px and above, they revert to a horizontal flex row. Safe area insets are applied via `@supports (padding: max(0px))`.
