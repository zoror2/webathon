# CivicPulse — WEBNOVA 2026

A responsive, map-first civic reporting workspace for Bengaluru. Dark editorial dashboard, lime accents, animated issue markers, mobile navigation, and accessible native dialogs.

## Implemented

- Photo evidence (JPG/PNG/WebP up to 4 MB), text, browser speech-to-text, manual coordinates and geolocation.
- Explainable keyword-based category and severity triage; routing to six civic categories and five departments.
- Same-category active cases within 100 metres are grouped; duplicate evidence and report counts are retained.
- Interactive Leaflet map, report-density circles, case filtering, derived statistics, category breakdown, department workloads.
- Case IDs, tracking, status editing through Reported / Assigned / In Progress / Resolved.
- Server persistence and image uploads using a Cloudflare R2 BUCKET binding. No API keys are shipped to the browser.
- Twelve clearly labeled sample cases to make the competition demo immediately explorable.

## Run locally

Requires Node.js 20+.

```sh
npm run build
npm run dev
```

Visit http://localhost:3000. Local data is stored in `.local-data/`. No dependency install is required for the local server. Map tiles, Leaflet, and fonts load from their respective CDNs.

## Deploy

The build emits `dist/server/index.js`, a Cloudflare Workers ES module exporting `fetch`. Bind an R2 bucket as `BUCKET`. The included Sites manifest declares this binding. A `wrangler.jsonc` example is included; replace the bucket name with your own before `npx wrangler deploy`.

## Honest demo boundaries

This is a competition prototype, not an operational municipal portal. Triage is deterministic text rules, **not a trained AI model**. Uploaded images are stored as evidence and are not automatically classified. Speech recognition requires a supported browser (usually Chrome) and microphone permission. Hotspots are proportional geographic circles, not a statistical density estimate. Sample records are fixtures; changes are persisted as overrides. The application has no user login or authority authentication; everyone with access to this demo can update a case. Do not collect personal or sensitive information in it.

R2 JSON objects are simple prototype persistence. Concurrent duplicate submissions or updates can race; use transactional SQL storage and protected role-based endpoints before public production. Duplicate detection is approximate (category + 100 m) and may group separate nearby issues. Auto-routing is a workspace assignment, not delivery to actual government systems.

## Verification

`npm test` checks classification, distance calculations, create/duplicate/status flows, rejected input, and evidence persistence against an in-memory R2 test adapter. `npm run validate` verifies the built Worker artifact. Browser visual QA is not included in these checks.

## Credits

Map: OpenStreetMap contributors / CARTO. Mapping UI: Leaflet. Fonts: DM Sans and Space Grotesk (Google Fonts).
