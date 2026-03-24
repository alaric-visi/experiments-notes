# Phonetic Alphabet

## Overview
This project converts freeform text input into a word-by-word phonetic spelling using a British-inflected variant of the NATO phonetic alphabet. The conversion is reactive and displays inline alongside a static reference grid.

## Core Mechanisms

### Lookup Table
The alphabet is stored in a plain object `ukPhonetic` mapping each uppercase letter `A`–`Z` and digit `0`–`9` to its spoken equivalent. The table diverges from standard NATO in several positions, replacing internationally standardised words with British civilian equivalents: `Freddie` for F, `London` for L, `Monkey` for M, `Robert` for R, `Sugar` for S, and `Tommy` for T. Digits use plain cardinal names.

### Conversion Logic
`convertToPhonetic(text)` iterates over `text.toUpperCase()` character by character. Each character is looked up in `ukPhonetic`:
- If found, it renders as a `<span>` pairing the bold highlighted character with its phonetic word.
- A space character renders as the literal string `[space]`.
- A newline character inserts a `<br>` element.
- Any character not present in the table (punctuation, symbols) is rendered as-is without a phonetic word.

The array of HTML strings is joined and assigned to `output.innerHTML`. The function returns an empty-state message if the input contains only whitespace.

### Reference Grid
On page load, `Object.entries(ukPhonetic)` is iterated and a `<div>` element is appended to `#reference` for each entry. The grid uses CSS `auto-fill` with a 120 px minimum column width, causing it to reflow automatically at narrow viewport widths.

### Reactive Update
An `input` event listener on the textarea calls `convertToPhonetic` with the current value and writes the result to `output.innerHTML` synchronously on every keystroke.
