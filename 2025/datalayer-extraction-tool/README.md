# DataLayer Extraction Tool

## Overview
This project presents an interactive web utility designed to simulate the extraction of `window.dataLayer` objects and Schema.org structured data from specified URLs. The interface features a dark-themed, dashboard-style layout complete with status indicators, loading states, and a simulated extraction log.

## Core Mechanisms

### Simulated Headless Extraction
The tool provides a "Run DataLayer Scan" feature which uses asynchronous delays to mimic a headless browser lifecycle (loading content, executing scripts, and capturing dynamic events). It programmatically injects typical Google Tag Manager (GTM) payloads and `pageview` push events into a results array.

### Schema.org Parsing
The "Run Schema Scan" function mimics fetching raw HTML via a CORS proxy. It uses regular expressions to parse the document for standard semantic data formats:
- **JSON-LD**: Extracts and parses contents from `<script type="application/ld+json">` tags.
- **Microdata & RDFa**: Scans for `itemtype` and `typeof` attributes to detect embedded schema representations.

```javascript
const jsonLdPattern = /<script[^>]*type\s*=\s*["']application\/ld\+json["'][^>]*>([\s\S]*?)<\/script>/gi;
// Executed against fetched HTML to extract structured JSON data
```

### Data Visualisation
Extracted items are dynamically rendered into formatted DOM elements, displaying the detection method used (e.g., JSON-LD or GTM Container), the status of the extraction, and the raw JSON payload in a formatted code block.
