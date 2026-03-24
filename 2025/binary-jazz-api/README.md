# Binary Jazz API Interface

## Overview
This project provides an interface for interacting with the Binary Jazz API to generate random, fictional musical genres. The application allows users to discover new genre names with a single click.

## Core Mechanisms

### Asynchronous Data Fetching
The application uses the Fetch API to asynchronously request genre data from the Binary Jazz external endpoint. Error handling is included so that if the endpoint fails to return data, the user is presented with a fallback message.

```javascript
async function fetchGenre() {
    // ...
    try {
        const res = await fetch("https://binaryjazz.us/wp-json/genrenator/v1/genre/1/");
        if (!res.ok) throw new Error(`API error: ${res.status}`);
        
        const data = await res.json();
        const genre = Array.isArray(data) ? data[0] : data;
        
        // Update DOM
        // ...
    } catch (err) {
        // Fallback handling
    }
}
```

### State Feedback
The application provides visual feedback on its current state. A status indicator updates to reflect whether the application is loading, has completed a request, or has encountered an error. Button states are managed to prevent concurrent requests whilst a fetch is in progress.

### Interaction
The interface accepts standard keyboard events (`Enter` or `Space` on the focused button) for operability. CSS transitions are used between subsequent API loads as the text updates.
