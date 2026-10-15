# Astronomy Picture (NASA APOD)

Shows NASA's Astronomy Picture of the Day full-bleed, with title, date, and
explanation. Updates daily.

## How it works

The widget does everything itself. There is no daemon and it writes no files:

- It reads NASA's APOD Basic JSON feed (`science.nasa.gov/wp-json/wp/v2/apod-basic`,
  no API key) through the widget proxy.
- The feed is newest-first. It shows the first entry that is a video (title and
  explanation only, with a pointer to apod.nasa.gov) or an image that actually loads;
  a logo placeholder or a tiny image is skipped in favour of the previous day.
- The image is loaded straight from NASA's CDN as an `<img>`; NASA's resizable image URLs are asked for at up to 1600px wide.
  Non-ASCII characters in image URLs (e.g. "č") are percent-encoded.
- The last good entry is kept in `localStorage` (when the sandbox allows it) and
  shown if the feed is unreachable on a fresh start.

Vardek refreshes the widget every 30 minutes (`refreshInterval`), and a failed first load retries after a minute.

## Why no daemon

Vardek asks you to re-approve a widget whenever any file in its folder changes, and a
daemon rewrites `data.js` on every update. Keeping the folder static means you approve
once. (The proxy returns text only, so image bytes can't be fetched through it; a plain
cross-origin `<img>` does load.)

## Permissions

`proxy`: `science.nasa.gov` (feed) and `assets.science.nasa.gov` (images).

## Settings

**Show title & explanation** (default on), **Ken Burns pan/zoom** (default on).
