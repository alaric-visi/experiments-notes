# Daily Horoscope

## Overview
This application is an engaging, interactive daily horoscope reader. Sporting a sleek, dark-themed UI with clean typography and fluid animations, users can select their astrological sign from a responsive grid to receive bespoke daily insights. 

## Core Mechanisms

### Dynamic Zodiac Grid
The interface features an interactive grid of zodiac signs constructed programmatically from a constant array of objects. Each object dictates the sign's styling, icon (`font-awesome`), and relevant date range:

```javascript
const zodiacSigns = [
    { name: "Aries", value: "aries", icon: "fa-fire", dates: "Mar 21 - Apr 19" },
    // ...
];
```

### API Integration and Proxies
The application fetches live astrological data via an external Horoscope API. To circumvent CORS policy restrictions commonly found on client-side fetching from simple APIs, it utilises `corsproxy.io` as a secure conduit for the endpoint request.

```javascript
const proxyUrl = "https://corsproxy.io/?";
const targetUrl = `https://horoscope-app-api.vercel.app/api/v1/get-horoscope/daily?sign=${selectedZodiac.value}&day=today`;
const apiUrl = proxyUrl + encodeURIComponent(targetUrl);
```

### Robust Fallback System
In the event that the external API rate-limits the client or suffers downtime, the application employs a graceful degradation strategy. It automatically falls back to a curated subset of locally stored "sample" horoscopes for each sign, guaranteeing that users still receive a reading and a functional experience regardless of external network issues.
