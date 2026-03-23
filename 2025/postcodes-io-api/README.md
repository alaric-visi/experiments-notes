# Postcodes.io API Explorer

## Overview
This project serves as a comprehensive visual dashboard and front-end interface for querying the public [postcodes.io](https://postcodes.io) REST API. The interface allows users to execute various geographic data queries—such as postcode lookups, validation, and coordinate-to-postcode translation—without requiring command-line tools like curl.

## Core Mechanisms

### Asynchronous Fetch Wrappers
The core interaction with the external API is handled by distinct wrapper functions around standard JavaScript `fetch()` calls. It differentiates between simple `GET` requests (for basic lookups) and `POST` requests (for bulk operations where multiple items evaluate in one go).

```javascript
async function getJSON(url) {
    const response = await fetch(url);
    if (!response.ok) {
        throw new Error(`${response.status} ${response.statusText}`);
    }
    return await response.json();
}

async function postJSON(url, body) {
    const response = await fetch(url, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(body)
    });
    // ...
}
```

### Smart Dispatcher (Global Search)
The application includes a global search bar that uses client-side Regular Expressions to automatically infer whether the user has input a full postcode, an outcode (the first half of a postcode), or a plain text place query. It then routes the request to the correct specific function.

```javascript
document.getElementById("globalSearchBtn").addEventListener("click", () => {
    const query = document.getElementById("globalSearch").value.trim();
    if (!query) return;

    if (query.match(/^[A-Z]{1,2}\d[A-Z\d]? ?\d[A-Z]{2}$/i)) {
        // Matches Full UK Postcode
        document.getElementById("postcodeInput").value = query;
        lookupPostcode();
    } else if (query.match(/^[A-Z]{1,2}\d[A-Z\d]?$/i)) {
        // Matches Outcode Only
        document.getElementById("outcodeInput").value = query;
        lookupOutcode();
    } else {
        // Fallback to text query
        document.getElementById("placeQueryInput").value = query;
        queryPlace();
    }
});
```

### Recursive Data Rendering
Because the API returns deeply nested JSON objects varying wildly by endpoint, the application uses a recursive `generateHTML()` function to procedurally build an unordered list `<ul>` structure. This ensures scalar values, embedded arrays, and key-value objects are visually formatted regardless of the schema structure received.
