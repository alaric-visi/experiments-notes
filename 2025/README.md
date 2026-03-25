# 2025

A collection of browser-based experiments and utilities produced during 2025. Each project is a self-contained directory containing one or more HTML files. Projects span canvas games, Three.js visualisations, API integrations, generative art, and interactive tools.

## Projects

### Games

| Directory | Description |
|-----------|-------------|
| [`apache-game`](./apache-game/) | Vertically-scrolling canyon-flight game with a phosphor-green CRT aesthetic, procedural canyon generation via superimposed sine waves, three enemy types, and a homing missile system |
| [`asteroids-balls`](./asteroids-balls/) | Canvas-based physics simulation where bouncing balls collide with destructible asteroids |
| [`asteroids-game`](./asteroids-game/) | Multi-level Asteroids implementation with per-level asteroid silhouette variants, fragmentation trees, and a dual-canvas starfield background |
| [`maze`](./maze/) | Interactive maze rendered on Canvas, solved using an A* pathfinding algorithm with animated traversal |
| [`psycho-killer-chicken-dinner`](./psycho-killer-chicken-dinner/) | Canvas game |
| [`space-invaders`](./space-invaders/) | Canvas implementation of the Space Invaders arcade game with wave-based enemy descents |
| [`tokyo-runner`](./tokyo-runner/) | Cyberpunk-themed text adventure set in a near-future Tokyo |

---

### Three.js Visualisations

| Directory | Description |
|-----------|-------------|
| [`anas`](./anas/) | Wireframe pterosaur skeleton assembled from Three.js primitives (spheres, cones, cylinders, and `ShapeGeometry` membranes), rotating continuously with a sinusoidal pitch oscillation |
| [`architectural-wireframes`](./architectural-wireframes/) | Wireframe architectural structures rendered in Three.js |
| [`crystal-cave`](./crystal-cave/) | Procedurally-generated Three.js crystal formations inside a cave environment |
| [`dodecahedron`](./dodecahedron/) | Animated Three.js dodecahedron with configurable material and lighting |
| [`fluid-background`](./fluid-background/) | WebGL fluid simulation using a multipass GLSL shader pipeline with advection, pressure solve, and divergence correction |
| [`hypersphere`](./hypersphere/) | Four-dimensional hypersphere projected into 3D and rendered in Three.js with interactive rotation |
| [`julia-set`](./julia-set/) | Real-time Julia set renderer using GLSL fragment shaders, with mouse-driven parameter adjustment |
| [`phases-1-through-4`](./phases-1-through-4/) | Four sequential mathematical visualisations exploring wave interference, Lissajous figures, and related periodic functions |
| [`polytope`](./polytope/) | Three.js rendering of a regular polytope with configurable symmetry group |
| [`threejs-experiments`](./threejs-experiments/) | Collection of standalone Three.js rendering experiments |
| [`vortex-svg`](./vortex-svg/) | Animated SVG vortex produced by parametric path generation |

---

### Generative Art and Animation

| Directory | Description |
|-----------|-------------|
| [`after-truth-comes-advertising`](./after-truth-comes-advertising/) | Generative text and visual composition exploring advertising language |
| [`constants`](./constants/) | Canvas visualisation of mathematical constants rendered as geometric patterns |
| [`geometries`](./geometries/) | Eight WebGL geometry experiments: two Glome projections, Klein bottle, Lemniscate of Bernoulli, two Quartic Plane Curves, and two Tesseract projections |
| [`ghost-of-molly-roger`](./ghost-of-molly-roger/) | Canvas-based animated scene |
| [`leo`](./leo/) | EchoLogix Terminal — a styled command-line interface for the EchoLogix narrative project |
| [`mosaic`](./mosaic/) | Canvas mosaic tile generator producing randomised colour grids |
| [`pocket-tiles`](./pocket-tiles/) | Procedurally-arranged tile grid with configurable colour schemes |
| [`pulse`](./pulse/) | Capital and Subjectivity Terminal — a generative text interface producing critical-theory-inflected output |
| [`seven-sins`](./seven-sins/) | Visual representation of the seven deadly sins |
| [`sine-wave-button`](./sine-wave-button/) | UI component experiment in which a button's border traces an animated sine wave |
| [`strudel-loop`](./strudel-loop/) | Algorithmic music sequencer using the Strudel live coding environment |
| [`webpage-background`](./webpage-background/) | Canvas fluid-animation background using particle advection, intended for embedding in other pages |

---

### API Integrations

| Directory | Description |
|-----------|-------------|
| [`binary-jazz-api`](./binary-jazz-api/) | Queries the Binary Jazz API to generate random musical genre combinations |
| [`colormind-api`](./colormind-api/) | Fetches harmonious colour palettes from the Colormind neural-network API and renders swatch previews |
| [`daily-horoscope`](./daily-horoscope/) | Retrieves and displays a daily horoscope via a public horoscope API |
| [`hostio-api`](./hostio-api/) | Domain intelligence dashboard querying the Host.io `/api/full/` endpoint, displaying web metadata, DNS records, IP information, and related domains across categorised cards |
| [`html-to-markdown-jina-ai-api`](./html-to-markdown-jina-ai-api/) | Fetches a remote URL via the Jina AI Reader API and returns its content as Markdown, with a wave-animated canvas background |
| [`philosphers-on-linkedIn`](./philosphers-on-linkedIn/) | Satirical generator producing LinkedIn-style posts written in the voice of historical philosophers |
| [`postcodes-io-api`](./postcodes-io-api/) | Queries the Postcodes.io API to retrieve geographic and administrative data for UK postcodes |
| [`reddit-json-extractor`](./reddit-json-extractor/) | Appends `.json` to Reddit URLs and parses the response to extract and display post and comment data |
| [`robohash-api`](./robohash-api/) | Generates unique robot avatars via the RoboHash API from arbitrary text input |
| [`wayback-machine`](./wayback-machine/) | Queries the Wayback Machine Availability API to retrieve the closest archived snapshot for a given URL |

---

### Text and Data Tools

| Directory | Description |
|-----------|-------------|
| [`datalayer-extraction-tool`](./datalayer-extraction-tool/) | Parses `dataLayer` push objects from pasted page source or a live URL, displaying each event key and value in a structured table |
| [`html-entity-escaper`](./html-entity-escaper/) | Converts HTML special characters to their named or numeric entity references, and reverses the process |
| [`keyword-extractor`](./keyword-extractor/) | Analyses pasted text to produce a ranked frequency list of extracted keywords, with configurable stop-word filtering |
| [`morse-code`](./morse-code/) | Bidirectional translator between plain text and International Morse Code, with optional audio tone generation |
| [`phonetic-alphabet`](./phonetic-alphabet/) | Converts plain text to NATO phonetic alphabet equivalents, one word per character |
| [`prism-js`](./prism-js/) | Code syntax highlighting demonstration using the Prism.js library across multiple language grammars |
| [`semaphore`](./semaphore/) | React 18 application that converts text into animated railway semaphore flag diagrams rendered as SVG |
| [`url-extractor`](./url-extractor/) | Extracts all URLs from pasted text or HTML source using regular expression matching |

---

### Narrative and Experimental

| Directory | Description |
|-----------|-------------|
| [`colour-palette-generator`](./colour-palette-generator/) | Generates harmonious colour palettes using HSL arithmetic and renders exportable CSS variable sets |
| [`echologix`](./echologix/) | Multi-page narrative project comprising a main story (`fragments-of-finn.html`), a video archive (`videos.html`), and an editorial layer (`stories.html`) |
