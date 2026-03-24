# Fluid Background

## Overview
This project implements a real-time incompressible fluid simulation rendered on an HTML5 Canvas via WebGL. The solver follows the Navier-Stokes equations on a staggered grid using operator splitting: advection, vorticity confinement, divergence computation, pressure iteration, and gradient subtraction. Two optional post-processing passes — bloom and sunrays — are composited on top of the dye field before display.

## Core Mechanisms

### WebGL Context and Texture Format Detection
`getWebGLContext` first attempts a WebGL 2 context, falling back to WebGL 1. On WebGL 2, `EXT_color_buffer_float` is requested to enable float framebuffer attachments; on WebGL 1, `OES_texture_half_float` and `OES_texture_half_float_linear` are used instead. `getSupportedFormat` probes each candidate internal format by attaching it to an FBO and calling `checkFramebufferStatus`, falling back through `R16F → RG16F → RGBA16F` until a renderable format is found.

### Shader Programs and Material System
All GLSL shaders are compiled at startup. The `Program` class wraps a linked shader programme and caches all active uniform locations by name. The `Material` class extends this to support multiple keyword-defined variants of a single fragment shader: `setKeywords` accepts an array of `#define` strings, computes a hash of the combined keywords, lazily compiles the keyword variant only once, and activates the corresponding programme. This is used by `displayMaterial` to conditionally compile in the `SHADING`, `BLOOM`, and `SUNRAYS` code paths.

### Double Framebuffer (Ping-Pong)
`createDoubleFBO` allocates two single FBOs of identical dimensions and wraps them in an object exposing `read` and `write` accessors with a `swap()` method. Each simulation field that requires in-place update (velocity, dye, pressure) is stored as a double FBO. After each compute pass writes into `write`, `swap()` is called so the result becomes `read` for the next pass.

### Simulation Step (`step`)
Each frame advances the simulation through six sequential GPU passes using `blit`, which draws a full-screen quad into the target FBO:

1. **Curl** — Computes the vorticity scalar field from the four-neighbour velocity differences using the `curlShader`.
2. **Vorticity confinement** — Reads curl magnitude at neighbouring cells, computes a normalised confinement force proportional to `config.CURL`, and adds it to the velocity field via the `vorticityShader`.
3. **Divergence** — Computes the scalar divergence of the velocity field with free-slip boundary conditions: samples outside [0,1] are reflected as negated components.
4. **Pressure initialisation** — Multiplies the previous pressure field by `config.PRESSURE` (0–1) using the `clearShader`, retaining a fraction of the prior solve to warm-start the Jacobi iteration.
5. **Pressure Jacobi iteration** — Runs `config.PRESSURE_ITERATIONS` (default 20) passes of the Jacobi solver, each time reading from `pressure.read` and writing into `pressure.write` and swapping.
6. **Gradient subtraction** — Subtracts the pressure gradient from the velocity field to enforce the divergence-free constraint.
7. **Advection** — The advection shader performs semi-Lagrangian advection: for each texel it looks up the velocity at that position, steps backward by `dt * velocity`, and samples the source field at that back-traced coordinate. When `OES_texture_float_linear` is unavailable, a manual bilinear interpolation kernel (`bilerp`) is used instead. Advection is applied first to the velocity field (self-advection), then to the dye field. Each advected value is divided by `1 + dissipation × dt` to model exponential decay.

### Splat Injection
`splat(x, y, dx, dy, color)` runs the `splatShader`, which adds a Gaussian blob centred at the normalised screen position `(x, y)` to both the velocity and dye fields. The Gaussian radius is corrected for aspect ratio. `multipleSplats` generates a random number of splats with colours from `generateColor` and random velocities of magnitude 1000 to seed the initial state.

### Colour Generation
`generateColor` converts HSV to RGB using a standard six-sector formula. In `RANDOM_COLORS` mode a random hue is chosen each call; otherwise `config.SPLAT_HUE` is used. The RGB values are multiplied by 0.15 to keep the injected dye at a low initial intensity, preventing immediate saturation.

### Bloom Post-Processing
`applyBloom` implements a dual-threshold, iterative down/up sample blur:
1. `bloomPrefilterShader` applies a soft-knee threshold curve, allowing frequencies above `BLOOM_THRESHOLD` to pass whilst smoothly attenuating the knee region.
2. `bloomBlurShader` downsamples into a mip chain of `BLOOM_ITERATIONS` half-resolution FBOs using a 4-tap box blur via the staggered `vL`, `vR`, `vT`, `vB` varyings on the blur vertex shader.
3. The same shader upsample-accumulates back up the chain with additive blending enabled.
4. `bloomFinalShader` multiplies the result by `BLOOM_INTENSITY` and writes to the bloom FBO.

In the display pass, the bloom texture is dithered with a 64×64 random luminance texture (`createDitheringTexture`) scaled to cover the display, then gamma-corrected via `linearToGamma` (approximation of `pow(color, 0.4167)`) before being added to the scene colour.

### Sunrays
`applySunrays` first runs `sunraysMaskShader`, which maps pixel brightness to an alpha mask (bright regions become transparent). `sunraysShader` then performs a 16-iteration radial sample from the fragment's UV coordinate towards the screen centre, accumulating the mask value with exponential decay (`Decay = 0.95`, `Density = 0.3`, `Exposure = 0.7`). The result is blurred once with `blur()` before being applied as a multiplicative factor in the display pass.

### Input Handling
Mouse and touch events translate cursor positions to normalised texture coordinates, computing `deltaX` and `deltaY` relative to the previous position. `correctDeltaX` and `correctDeltaY` normalise the deltas so diagonal motion appears uniform regardless of aspect ratio. The spacebar pushes a random splat count onto `splatStack`, which `applyInputs` pops and passes to `multipleSplats` on the next frame.
