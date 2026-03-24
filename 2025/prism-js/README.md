# Prism.js Live Theming

## Overview
This project demonstrates dynamic integration with Prism.js syntax highlighting. It allows users to switch between different syntax themes (such as Default, Okaidia, and Dark) as well as functional plugins (such as Line Numbers) using a dropdown menu without reloading the page.

## Core Mechanisms

### Dynamic Resource Loading
To swap themes, the application uses JavaScript to inject and replace stylesheets and script tags as needed. The project uses CDN-hosted Prism.js assets rather than local files.

```javascript
function loadBundle(bundleId) {
    const bundle = bundles[bundleId];
    if (!bundle) return;
    
    cleanupPrismAssets(); // Removes old styles and scripts
    
    // Inject selected CSS
    // Inject Core JS and wait for it to load
    // Conditionally load plugins (e.g., line-numbers)
}
```

### Plug-in Injection Pipeline
The script loads dependencies exactly when required, such as the `line-numbers` plugin. The loader ensures that the Prism core executes before injecting child plugins to prevent undefined references during syntax parsing.

### Styling
The application is styled with a dark background and a static grid overlay. Code blocks apply `transform` for hover states alongside the "Fira Code" monospace font for the rendered text.
