# Semaphore Translator

A React 18 application that converts plain text into animated railway semaphore flag diagrams, rendering each character sequentially via SVG.

## Overview

The component is delivered as a single HTML file that loads React 18, ReactDOM, and Babel Standalone from CDN. Babel transpiles the embedded JSX at runtime. Tailwind CSS governs layout and spacing; a small bespoke stylesheet handles the flag display grid, input field, and status indicator.

## Architecture

The application consists of a single functional component, `SemaphoreTranslator`. All state is managed through four `useState` hooks: `inputText`, `isAnimating`, `currentIndex`, and `displayText`.

## Core Mechanisms

### Lookup Table

`semaphorePositions` maps each lowercase letter and the space character to a two-element array of degree values representing the angles of the left and right flags respectively. Angles follow a standard semaphore convention, measured in degrees clockwise from the three o'clock axis. The space character maps to `[0, 0]`, placing both flags horizontally to the right, which conventionally signals a word break.

### Translation and Input Sanitisation

`handleTranslate` strips all characters that are not lowercase letters or spaces using the regular expression `/[^a-z ]/g`. The cleaned string is written to `displayText`, `currentIndex` is reset to zero, and `isAnimating` is set to `true`.

### Sequential Animation

A `useEffect` hook with dependencies `[currentIndex, isAnimating, displayText]` drives the reveal. When `isAnimating` is true and `currentIndex` is less than `displayText.length`, a `setTimeout` of 200 ms schedules incrementing `currentIndex`. The cleanup function cancels a pending timeout if the component re-renders before the timer fires. When `currentIndex` reaches the end of the string, `isAnimating` is set to `false`.

### SVG Flag Rendering

`renderSemaphore` accepts a single character and retrieves its angle pair from the lookup table. It renders a 100×100 viewBox SVG containing a central pivot circle and two calls to `renderFlag`.

`renderFlag` receives an angle in degrees and a side identifier. It converts the angle to radians and computes the tip coordinates using `cx + length × cos(rad)` and `cy + length × sin(rad)`, where `cx = cy = 50` and `length = 35`. A `<line>` element draws the staff from the pivot to the tip. A `<rect>` element is placed at the tip and rotated by `angle + 90` degrees around the tip coordinates, aligning the flag fabric perpendicular to the staff. Left flags are rendered in blue (`#2563eb`) and right flags in red (`#dc2626`).

### Display Grid

The `flag-display-grid` CSS class uses `repeat(auto-fill, minmax(60px, 1fr))` to produce a responsive grid of square cells. Each cell is assigned either `flag-item-active` or `flag-item-inactive` based on comparison of its index against `currentIndex`. Inactive cells display a placeholder dot; active cells render the SVG diagram. The transition between states is governed by a 100 ms CSS `transition` on the border and background properties.

### Status Indicator

An eight-pixel circular element changes class between `status-active` (white with a box shadow glow) and `status-inactive` (grey) based on `isAnimating`. Alongside it, a status string reports the transmission progress as `TRANSMITTING: n/total`, `TRANSMISSION COMPLETE`, or `AWAITING INPUT`.
