# ColorMind API Interface

## Overview
This project is a tool that uses the Colormind API to generate colour palettes based on its models, as well as providing completely random colour combinations. It is designed for testing and generating colours for external projects.

## Core Mechanisms

### Data Fetching and Fallbacks
The application makes asynchronous `POST` requests to the Colormind API to retrieve arrays of RGB sequences. It implements error handling: if the API request fails or is blocked by network issues, it automatically falls back to a predefined local palette.

```javascript
try {
    const response = await fetch("http://colormind.io/api/", {
        method: "POST",
        body: JSON.stringify({ model: "default" })
    });
    // ...
} catch (error) {
    const fallbackPalette = [
        [255, 105, 54],
        [59, 29, 34],
        [39, 22, 30],
        [38, 14, 14],
        [233, 233, 240]
    ];
    displayPalette(fallbackPalette);
}
```

### Colour Conversion
The fetched JSON returns RGB arrays which must be converted to hex codes for display and clipboard operations. A bitwise operator function performs this conversion:

```javascript
function rgbToHex(r, g, b) {
    return "#" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1).toUpperCase();
}
```

### DOM Manipulations
A grid system injects `color-card` elements populated with the requested colours. Clicking on a card copies the hex code to the user's clipboard and displays a notification. Cards use CSS transitons during their initial render to sequence their appearance.
