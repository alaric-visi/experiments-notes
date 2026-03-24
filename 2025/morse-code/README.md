# Morse Code

## Overview
This project translates freeform text input into international Morse code and plays it back as an audible tone sequence using the Web Audio API. The translation and playback are handled entirely in the browser with no external dependencies.

## Core Mechanisms

### Lookup Table
The character-to-Morse mapping is stored in a plain object literal `morseCode`, covering the 26 Latin letters, digits 0–9, and a set of punctuation characters. Each key is an uppercase character string and each value is the corresponding ITU-R M.1677 sequence of dots and dashes. Characters not present in the table are silently skipped during translation.

### Timing Constants
Timing follows the standard Morse code element ratio. A dot duration of 80 ms is defined as `dotDuration`. All other durations derive from this:

```javascript
const dashDuration = dotDuration * 3;   // 240 ms
const symbolGap   = dotDuration;        // 80 ms
const letterGap   = dotDuration * 3;    // 240 ms
const wordGap     = dotDuration * 7;    // 560 ms
```

A fixed oscillator frequency of 600 Hz is used throughout.

### Audio Generation
`playTone(duration)` returns a `Promise` that resolves after the given duration in milliseconds. Each call creates a fresh `OscillatorNode` connected to a `GainNode`, which connects to `audioContext.destination`. The oscillator is set to `sine` type and started immediately. The gain is set to 0.3 at the current time and ramped exponentially to near-zero using:

```javascript
gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + duration / 1000);
```

This produces a natural click-free decay at the end of each tone. The oscillator is scheduled to stop at the same time. A `setTimeout` inside the promise resolves it after the full duration, ensuring sequential execution.

### Playback Sequencer
`playMorseCode(text)` is an `async` function that iterates over the input word by word, then character by character, then symbol by symbol. For each dot or dash it appends the character to the displayed output element and calls `playTone`, `await`-ing its resolution before proceeding. Inter-symbol gaps, inter-letter gaps, and inter-word gaps are inserted using `await sleep(ms)`, a helper that wraps `setTimeout` in a `Promise`. Three space characters are appended to the output between words to represent the word gap visually.

The `isPlaying` flag and the `disabled` attribute on the button prevent re-entrant invocations during playback.

### User Interaction
A `click` listener on the translate button triggers `playMorseCode` after trimming the input. A `keypress` listener on the textarea triggers the same action when Ctrl+Enter is pressed. The `AudioContext` is constructed at page load; browsers that require user activation before audio playback will defer actual sound output until the button is first clicked.
