# Keyword Extractor

## Overview
This project fetches the HTML of any publicly accessible webpage via CORS proxies and analyses its text content to produce three classes of output: high-frequency individual keywords, frequent multi-word phrases (bigrams and trigrams), and latent semantic keywords identified through co-occurrence. All processing runs in the browser with no server-side component.

## Core Mechanisms

### CORS Proxy Chain
`fetchWithProxy` iterates over four proxy strategies in order, aborting each attempt after 15 seconds via `AbortController`. The fourth entry is a function that constructs the `allorigins.win/get` JSON endpoint, whose response is unwrapped as `data.contents` rather than read as raw text. Attempts continue to the next proxy on any non-OK HTTP status, timeout, or empty response. If all four fail, the function throws.

### Content Extraction (`extractTextFromHTML`)
The raw HTML string is parsed with `DOMParser`. Script, style, noscript, iframe, SVG, nav, header, and footer elements are removed before any text extraction. The function attempts a priority-ordered series of selectors targeted at Wikipedia's article structure (`#mw-content-text`, `.mw-parser-output`, etc.) before falling back to generic content selectors (`article`, `main`, `.content`, etc.). If no selector yields more than 1,000 characters, it scans all `p`, `div`, `section`, and `article` elements and selects the one with the largest text content. A final fallback reads `document.body.textContent`. The extracted string is post-processed to remove citation brackets (`[42]`), strip parenthetical asides, and collapse whitespace.

### Tokenisation and Stop Word Removal
`tokenizeText` lowercases the text, strips all non-alphanumeric characters except hyphens, and splits on whitespace. Tokens shorter than `minWordLength` and purely numeric tokens are discarded. A 70-entry `STOP_WORDS` `Set` of common English function words is applied as a further filter when `removeStopwords` is enabled.

### Porter Stemmer
`porterStemmer` implements a partial Porter algorithm. It applies Step 1a suffix rules for plurals (`sses → ss`, `ies → i`, trailing `s`), then Step 1b rules for past tense and gerunds (`ed`, `edly`, `ing`, `ingly`), including the double-consonant and short-word suffixation sub-rules. It does not implement Steps 2 through 5, making it a conservative stemmer that handles the most frequent morphological variations without over-truncating.

### N-gram Extraction and Frequency Counting
`extractNGrams(tokens, n, minWordLength)` produces all contiguous token sequences of length `n` in which every constituent word meets the minimum length requirement. `calculateKeywordScores` reduces any token array to a frequency map. Bigrams and trigrams are concatenated before frequency counting, producing a unified phrase set.

### Latent Keyword Discovery
`findLatentKeywords(keywords, allWords, windowSize, minWordLength)` builds a co-occurrence map. For each position in `allWords` whose token appears in the top 20 high-frequency keywords, it counts every distinct non-keyword neighbour within `±windowSize` tokens. The neighbours are sorted by co-occurrence count and the top 15 are returned. This approximates a simplified distributional semantics approach, surfacing words that are contextually adjacent to the primary keywords without being frequent enough to appear in the main list themselves.

### Display and Export
Results are rendered as styled tag elements with a small frequency badge. The three panels display high-frequency keywords (blue), key phrases (violet), and latent keywords (green). `copyResultsToClipboard` serialises all three lists as a numbered plain-text report and writes it to the clipboard via `navigator.clipboard.writeText`. A rate-limiter enforces a 2-second minimum between successive fetch requests using a `lastRequestTime` timestamp comparison.
