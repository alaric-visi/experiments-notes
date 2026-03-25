# Host.io API

A domain intelligence dashboard that queries the Host.io `/api/full/{domain}` endpoint and presents its response across four categorised cards: Web Metadata, DNS Records, IP Information, and Related Domains.

## Overview

The interface consists of a sticky header containing the domain input and a main content area that alternates between an initial-state prompt, a loading spinner, and the populated dashboard grid. The dashboard is a CSS `auto-fit` grid with a minimum column width of 350 px, collapsing to a single column on narrow viewports.

## Core Mechanisms

### API Request

`fetchHostIOData` constructs a request to `https://host.io{endpoint}` with a `Bearer` token supplied via the `TOKEN_PLACEHOLDER` constant. The call is dispatched through `window.proxyFetch` to support proxy injection. HTTP 404 responses are parsed as JSON to extract the `error` field from the Host.io error structure; HTTP 400 is mapped to an authentication failure message; all other non-OK statuses surface the HTTP status code directly.

The main entry point `fetchDomainData` strips `http://`, `https://`, `www.` prefixes and trailing slashes from the user input before constructing the endpoint path, normalising inputs such as `https://www.github.com/` to `github.com`.

### `createDataRow`

A shared DOM factory function used by all four render functions. It accepts a label string and a value that may be a primitive, an array, or an object:

- **Primitive:** assigned directly to `textContent`.
- **Array:** each element is rendered as a `.list-item` div inside a `DocumentFragment` to minimise reflows.
- **Object:** each key–value pair is rendered as a `.list-item` div using `innerHTML` with a `<strong>` label.

Both panels append their row elements to a `DocumentFragment` before replacing `innerHTML = ''` on the card content element, keeping DOM manipulation atomic.

### Web Metadata Card

`renderWebData` populates rows from `fullData.web`, including domain, global rank (prefixed with `#`), URL, IP, last scrape date, content size in KB (computed as `Math.round(data.length / 1024)`), server header, meta title, meta description, copyright, and external links. Fields that may be absent (`rank`, `copyright`, `server`, `links`) are conditionally appended.

### DNS Records Card

`renderDNSData` renders A records (IPv4), AAAA records (IPv6), MX records (mail exchangers), and NS records (nameservers) from `fullData.dns`. Each record type is conditionally appended only when the corresponding array is non-empty.

### IP Information Card

`renderIPData` iterates `Object.entries(fullData.ipinfo)`, which maps IP address strings to location and ASN objects. For each IP, a teal `<h4>` heading displays the address, followed by city, region, country, latitude/longitude coordinates, and timezone. If `info.asn` is present, a nested sub-section renders the ASN number, carrier name, domain, and IP route prefix on a slightly darker background.

### Related Domains Card

`renderRelatedData` iterates the `fullData.related` object, whose keys represent relationship types (e.g., backlinks, IP neighbours, name server co-hosted domains). For each type, a `<h4>` heading is rendered and each item in the array is presented as a `.related-item` tile showing the related domain value and a count badge styled in teal.

### Extended Intelligence Card

`renderExtendedData` serves as a data-coverage summary. It iterates four fixed data points (Web Metadata, DNS Records, IP Information, Related Domains) and renders each as a small grid cell containing the data type label and a check or cross icon, depending on whether the corresponding key in `fullData` is populated.

### Layout Adjustment

A `adjustLayout` function is registered on `window.resize` and called on load. It spans the Extended Intelligence card across two grid columns on viewports wider than 768 px, and resets it to a single column on narrower viewports, using `element.style.gridColumn`.

### Status Feedback

`showStatus` sets the text content of a status bar element and applies either `status-success` or `status-error` CSS classes. Success messages auto-dismiss after 3000 ms via `setTimeout`; error messages persist until the next action.

### Date Formatting

`formatDate` converts ISO date strings from the API to `en-GB` locale format using `toLocaleDateString` and `toLocaleTimeString`, producing output in the form `DD/MM/YYYY HH:MM`.
