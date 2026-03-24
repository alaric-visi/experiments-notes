# Robohash API Interface

## Overview
This project generates robotic avatars using the Robohash.org API. A button interaction triggers the generation of a fresh grid of procedurally created robot profiles.

## Core Mechanisms

### Hash-based Image Generation
The Robohash endpoint expects a unique string or number at the end of its URL to resolve the procedural generation logic. The script creates unique seed values by scaling `Math.random()` against a predetermined ceiling:

```javascript
function getRandomNumber() {
    return Math.floor(Math.random() * 1000000);
}
```
This random seed is parsed directly into the URL schema: `https://robohash.org/set_set2/bgset_bg1/${getRandomNumber()}?size=300x300`

### Asynchronous DOM Rendering
The grid initially populates with placeholder nodes containing a loading indicator. Image elements are appended to the DOM, but their visibility relies on the `img.onload` event handler replacing the loading spinner.

```javascript
const spinner = document.createElement("div");
// ... append to DOM
const img = new Image();
img.onload = () => {
    placeholder.innerHTML = "";
    placeholder.appendChild(img);
};
```

### Error Mitigation
If a network timeout or 404 occurs when requesting the avatar, an `img.onerror` fallback replaces the spinner with an error message string. This prevents the loading states from stalling indefinitely.
