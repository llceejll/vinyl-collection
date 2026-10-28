# Vinyl Collection

A single-page vinyl catalog app built with React (via CDN) and Tailwind CSS.

## What this project includes

- Search by artist or album title
- Filter by genre
- Filter by decade
- Album artwork cards with hover details
- Quick Spotify search links for each album

## Project structure

- `/index.html` — complete app (HTML, styles, and React code)
- `/.github/workflows/` — GitHub Actions workflows

## Run locally

Because this is a static app with CDN dependencies, no package installation is required.

1. Open `/home/runner/work/vinyl-collection/vinyl-collection/index.html` in a browser, or
2. Serve the directory with any static file server and open the served URL.

## Notes

- The app currently targets a curated 311-album dataset embedded directly in `index.html`.
- Workflow settings for Azure deployment should be updated with real app values and secrets before enabling deployment.
