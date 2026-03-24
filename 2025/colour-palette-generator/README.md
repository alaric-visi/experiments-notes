# Colour Palette Generator

## Overview
This application serves as a comprehensive tool to dynamically generate accessible, mathematically precise five-swatch colour palettes. By selecting a base colour, algorithmic colour theories are applied to extract complementary, analogous, triadic, or monochromatic schemes mapped directly to UI elements.

## Core Mechanisms

### Colour Space Conversion
To accurately compute geometric harmonies, the core algorithm transitions input Hex codes into the HSL (Hue, Saturation, Lightness) colour space. The script implements standalone mathematical structures to conduct `hexToRgb`, `rgbToHsl`, `hslToRgb`, and `rgbToHex` transforms, as direct manipulation of HSL channels facilitates predictable and visually cohesive rotational colour shifts over standard RGB space.

### Harmony Formulation
The `generateHarmoniousPalette` function applies distinct mathematical rules to the base hue depending on the selected structural mode:
- **Complementary**: Offsets the primary hue by exactly 180°.
- **Analogous**: Extracts adjacent geometric hues at narrow -30° and +30° offsets.
- **Triadic**: Solves for coordinates evenly distributed across the 360° colour wheel (`+120°` and `+240°`).
- **Monochromatic**: Retains the exact base hue whilst stepping through lightness (`l`) and saturation (`s`) parameters exclusively.

### Interface Integration
The script procedurally populates the DOM with individual colour cards exhibiting consecutive CSS entrance animations via incremented `animationDelay` mapping. Furthermore, using the asynchronous `navigator.clipboard.writeText` API, clicking a generated colour swatch seamlessly copies its corresponding hexadecimal value directly to the user's system clipboard, emitting a temporary notification node.
