# HTML Entity Escaper

## Overview
This project provides a minimal browser-based tool for escaping the five characters that carry syntactic meaning in HTML. Input typed into the first textarea is escaped in real time and displayed in a read-only output textarea. A button copies the result to the clipboard.

## Core Mechanisms

### Escaping Function
`escapeHtml(text)` applies five sequential `String.prototype.replace` calls with regular expressions carrying the global flag, converting each dangerous character to its named or numeric HTML entity:

| Input | Output |
|---|---|
| `&` | `&amp;` |
| `<` | `&lt;` |
| `>` | `&gt;` |
| `"` | `&quot;` |
| `'` | `&#39;` |

The ampersand replacement is applied first to avoid double-escaping entities that are introduced by the subsequent passes.

### Reactive Output
An `input` event listener on the source textarea calls `escapeHtml` with the current value and writes the result directly to `output.value`. No debouncing is applied; escaping runs synchronously on every keystroke.

### Clipboard Copy
The copy button's `click` handler calls `navigator.clipboard.writeText` with the escaped output. If the Async Clipboard API is unavailable or throws, the fallback selects the output textarea and issues `document.execCommand('copy')`. In both cases, the button text changes to `"Copied!"` and the background colour transitions to green for 1.5 seconds before reverting.
