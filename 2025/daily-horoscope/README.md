# Daily Horoscope

## Overview
This application is a daily horoscope reader. Users can select their astrological sign from a grid to receive daily insights based on their selection.

## Core Mechanisms

### Zodiac Grid
The interface features a grid of zodiac signs constructed programmatically from an array of objects. Each object dictates the sign's styling, icon, and relevant date range:

```javascript
const zodiacSigns = [
    { name: "Aries", value: "aries", icon: "fa-fire", dates: "Mar 21 - Apr 19" },
    // ...
];
```

### API Integration
The application fetches astrological data via an external Horoscope API. To bypass CORS policy restrictions on client-side fetching, it uses `corsproxy.io` as a conduit for the request.

```javascript
const proxyUrl = "https://corsproxy.io/?";
const targetUrl = `https://horoscope-app-api.vercel.app/api/v1/get-horoscope/daily?sign=${selectedZodiac.value}&day=today`;
const apiUrl = proxyUrl + encodeURIComponent(targetUrl);
```

### Fallback System
If the external API rate-limits the client or experiences downtime, the application falls back to a subset of locally stored sample horoscopes. This ensures that users receive text and the application remains functional regardless of external network issues.
