# Wayback Machine Analyser

## Overview
This project is an analytical tool designed to explore and visualise the evolution of a website's folder structure and content over time using historical data sourced from the Internet Archive's Wayback Machine.

## Core Mechanisms

### Archive Data Retrieval
The core functionality relies on querying the Internet Archive CDX API (`http://web.archive.org/cdx/search/cdx`). The application fetches historical coordinate data for a specified domain, filtering the payload for standard document formats (e.g. `text/html`) and successful HTTP status codes.

### Structural Visualisation
To translate raw snapshot data into meaningful trends, the application utilises the `Plotly.js` library. It analyses the hierarchical URL paths to aggregate directory structures and subsequently renders stacked line or bar charts to illustrate the growth and shifting prominence of top-level folders.

### Differential Analysis
The tool integrates the `diff_match_patch` library to facilitate comparative analysis between different temporal states of the website. This allows users to inspect specific page iterations and identify precise structural or textual modifications across the archive's history.
