# The Ghost of Molly Roger

## Overview
This project is a browser-based interactive narrative game set in Victorian England. It is built as a single HTML file with no external libraries beyond Font Awesome for iconography. The game state is driven by a flat JavaScript object containing all scenes and their transitions, with relationship tracking and a persistent save mechanism via `localStorage`.

## Core Mechanisms

### Scene Graph
All game content is stored in a single `scenes` object, keyed by scene ID. Each scene entry contains:

- `text` — the narrative passage displayed to the player.
- `choices` — an array of objects, each with a `text` label, a `nextScene` ID, and an optional inline `relationshipChanges` object that overrides the scene-level defaults for that specific choice.
- `relationshipChanges` — a default set of numeric deltas applied to the relationship scores when the scene is entered.
- `psychicChange` — a numeric delta applied to the psychic level on scene entry.

The graph is not enforced as a tree; a subset of scenes (endings) point back to `start`, creating a cycle. There are approximately 25 distinct scenes, with branching paths converging at several shared nodes such as `research_alone`, `grace_help`, and `approved_psychic`.

### State Management
Global state is maintained in three mutable variables: `currentScene` (a string ID), `relationshipScores` (an object mapping character names to numeric values), and `psychicLevel` (an integer). On each scene transition, `loadScene(sceneId)` is called, which:

1. Retrieves the scene object from `scenes`.
2. Applies `scene.relationshipChanges` to `relationshipScores`.
3. Increments `psychicLevel` by `scene.psychicChange`.
4. Updates the path classification string based on the accumulated Molly Roger and Worthington relationship values, using a set of threshold comparisons.
5. Delegates rendering to `renderScene`.

### Rendering
`renderScene(scene)` clears and repopulates the DOM:

- The `#scene-text` element receives the scene's text content and has a `typing` class applied temporarily to activate a CSS `::after` pseudo-element blinking cursor animation.
- The `#relationship-container` is rebuilt with a labelled progress bar for each tracked character, with bar widths calculated as `Math.max(0, Math.min(100, score))` percentage.
- The `#choices-container` is populated with a `<button>` for each choice. Each button's `click` handler calls `makeChoice(choice)`, which applies any choice-specific `relationshipChanges` before delegating to `loadScene`.

The typing effect is implemented with `setTimeout`-based character-by-character insertion into the scene text element, with each character appended at a 12 ms interval.

### Save and Restore
`saveGame()` serialises the three state variables into `localStorage` under the key `mollyRogerSave` using `JSON.stringify`. `loadGame()` restores them on page load using `JSON.parse`. If no saved state is present, the game begins at the `start` scene. A floating `#save-indicator` element briefly becomes visible (via CSS `display: flex`) on each save to confirm the action, then hides again after 2 seconds.

### Overlays
Two fixed-position overlays present supplementary content: an "About" overlay describing the game's narrative context, and a "Story" overlay reproducing the original SCP Foundation prose. Both overlays use `display: flex` toggled on the `.overlay` wrapper, with click listeners on the corresponding header links and close buttons. The `#story-link` button opens the story overlay; the `#about-link` button opens the about overlay.

### Attribution
The game is based on the SCP Foundation tale "The Ghost of Molly Roger" by psul, licensed under CC BY-SA 3.0. Attribution links are present in the page footer and within the story overlay.
