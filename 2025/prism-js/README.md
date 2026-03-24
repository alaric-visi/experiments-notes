# Prism.js Live Theming

## Overview
This project serves as a technical showcase for integrating Prism.js syntax highlighting with dynamic, live-theme switching. Aimed at documentation hubs or code-centric blogs, the interface enables users to instantly toggle between distinct syntax themes (such as Default, Okaidia, Dark) and functional plugins (Line Numbers) via an interactive dropdown.

## Core Mechanisms

### Dynamic Resource Loading
To swap themes efficiently without reloading the page, the application uses a bespoke JavaScript loading utility to inject and replace stylesheets and script tags sequentially. The project uses the CDN-hosted Prism.js assets to maintain a lightweight core.

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
The system can comfortably load complex dependent structures, such as the `line-numbers` plugin. The loader ensures that the overarching Prism core executes before attempting to inject child plugins, avoiding race conditions and undefined references during syntax parsing.

### Aesthetics and Presentation
To present the technical mechanism elegantly, the tool sits atop a glassy dark-mode backdrop featuring radial gradients and a subtle static grid overlay. Code blocks utilise scale and box-shadow hover states (`transform: translateY(-2px)`) alongside the "Fira Code" monospace font to create a satisfying reading experience.
