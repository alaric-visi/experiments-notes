# URL Extractor

## Overview
This project is a browser-based tool for fetching the HTML of any publicly accessible webpage and extracting every URL present within it. The extracted results can be filtered, sorted, deduplicated, and copied to the clipboard. Cross-origin requests are handled via a chain of third-party CORS proxies.

## Core Mechanisms

### CORS Proxy Fallback Chain
Because browser security policy prevents cross-origin `fetch` calls directly, the tool routes requests through a predefined list of three public CORS proxies:

```javascript
const CORS_PROXIES = [
    'https://api.allorigins.win/raw?url=',
    'https://corsproxy.io/?',
    'https://api.codetabs.com/v1/proxy?quest='
];
```

`fetchAndExtract()` iterates over this array in order, encoding the target URL and appending it to each proxy prefix. Each attempt is raced against a 15-second `Promise.race` timeout via `fetchWithTimeout`. If a proxy returns a non-OK HTTP status or the timeout elapses, the loop advances to the next proxy. If all proxies fail, an error is thrown with the last recorded error message.

### HTML Parsing and URL Extraction
`extractUrlsFromHTML(html, baseUrl)` uses `DOMParser` to convert the raw HTML string into a live document, then systematically queries it against a list of selector/attribute pairs:

| Selector | Attribute |
|---|---|
| `a[href]` | `href` |
| `img[src]`, `img[srcset]` | `src`, `srcset` |
| `link[href]`, `script[src]` | `href`, `src` |
| `iframe[src]`, `source[src]`, `video[src]`, `audio[src]`, `embed[src]` | `src` |
| `object[data]`, `form[action]` | `data`, `action` |
| `meta[content]` with OG/itemprop properties | `content` |
| `*[style*="url("]` | inline CSS |
| `*[data-src]`, `*[data-href]`, `*[data-url]` | data attributes |

`srcset` values are split on commas and each descriptor's URL portion is extracted separately. For inline style attributes, `extractFromStyle` applies a regex to capture the argument of any `url()` declaration.

A second pass applies a regex directly to the text content of all inline `<script>` and `<style>` elements, capturing absolute `https?://` URLs, `src=`, `href=`, and `url()` patterns that would not be reachable via DOM attributes.

### URL Normalisation
`cleanAndNormalizeUrl(url, base)` resolves all encountered URLs to absolute form:

- Protocol-relative URLs (`//example.com`) are prefixed with the base URL's protocol.
- Root-relative paths (`/path`) are prefixed with the base origin.
- Relative paths are resolved via `new URL(url, base.origin)`.

After resolving, any query parameters whose keys begin with `utm_`, `fbclid`, `gclid`, `msclkid`, `dclid`, `ref`, or `source` are deleted from the `URLSearchParams`. A trailing `/` is stripped. Fragment identifiers (`#…`) are discarded before resolution. `data:` URIs, bare fragment references, and `javascript:` or `mailto:` schemes are rejected and return `null`.

### URL Classification
`getUrlType(url, base)` determines whether a URL is internal or external by comparing its hostname to the base hostname, also accepting subdomains. It then inspects the pathname against extension patterns to assign a more specific type: `image`, `css`, `js`, `doc`, or the plain `internal`/`external` fallback. These type strings are stored on each URL record and used to drive badge rendering and filter controls.

### Filtering and Display
`extractedUrls` is a flat array of `{ url, type, original }` objects. `displayResults()` derives a filtered subset by applying, in order: a text filter against both the normalised URL and the original value, a type filter against the `type` string, and optionally a deduplication pass using a `Set`. The resulting list is rendered as a sequence of `.url-item` divs; each optionally includes a `.url-badge` span whose CSS class is chosen by `getBadgeClass`.

Filter inputs directly call `displayResults()` on every `input` or `change` event; they operate only on the in-memory `extractedUrls` array and do not trigger a new network request.

### Result Actions
- **Sort A-Z**: calls `Array.prototype.sort` with `localeCompare` on the normalised URL strings, then re-renders.
- **Remove Duplicates**: filters `extractedUrls` in-place using a `Set`, then re-renders.
- **Copy All**: writes the normalised URLs joined by newlines to the clipboard via `navigator.clipboard.writeText`.
- **Clear All**: empties `extractedUrls`, clears the input, and removes the last fetched URL from `localStorage`.

### Persistence and Iframe Support
The last successfully fetched URL is stored in `localStorage` under `lastFetchedUrl` and restored to the input field on load. The user's theme preference (dark/light) is similarly persisted under `theme` and applied by toggling a `light-mode` class on `<html>`.

If the page is running inside an `<iframe>` (detected by `window.self !== window.top`), a `MutationObserver` on `document.body` and a `resize` listener both post `iframeHeight` messages to the parent frame with the current `scrollHeight`, enabling the embedding page to resize the iframe dynamically.
