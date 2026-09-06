# F1 Circuit Walks

Free, fan-made walking guides to real F1 street circuits — with GPS tracking, historical facts, and offline support. Currently covers Monaco, Baku, Singapore, Las Vegas, and Melbourne.

**Live:** [f1walk.drumandbytes.dev](https://f1walk.drumandbytes.dev)

---

## What it is

A progressive web app (PWA) that guides you around a real F1 street circuit on foot, corner by corner. Each corner has:

- Apex speed, gear, and distance into the lap
- Description of the racing line and driving technique
- Historical facts, famous crashes, and circuit lore

Enable GPS and the app follows you automatically — it advances to the next corner as you approach. A landing page at the site root lets you pick a circuit, see it on a world map, and jump back and forth between circuits.

## Features

- **Live GPS tracking** — auto-advances to the next corner when you're within range, with anti-false-positive logic for corners that sit close together
- **Nearby highlight** — corner markers turn orange as you approach, giving you a heads-up before the panel switches
- **Auto-advance toggle** — turn automatic corner switching on/off via the Auto button
- **Offline support** — the app shell, visited map tiles, and an explicit "download map for offline use" button all work without signal after first load
- **Progress tracking** — visited corners and best lap time are saved locally (device-only, no account) and shown on both the circuit page and the hub's circuit cards
- **PWA** — install to home screen via Safari on iOS or Chrome/Brave on Android
- **Screen wake lock** — screen stays on automatically when GPS is active
- **Fullscreen mode** — available on Android and desktop (hidden on iOS where the API is not supported)
- **Cookieless** — GPS stays on your device, no cookies, no personal data collected; privacy-friendly Cloudflare Web Analytics (aggregate page views only)

## Installing as an app

**iOS:** Open in Safari → Share → Add to Home Screen
**Android:** Open in Chrome or Brave → install prompt or browser menu → Add to Home Screen

## GPS tracking

Uses the browser's built-in Geolocation API. Measures the distance to the next corner every few seconds (Haversine formula) and auto-advances once you're close enough and have moved past the previous corner. Battery/CPU impact is negligible — equivalent to having Maps open.

## Files

The site is generated: `templates/circuit.html` is the shared per-circuit template, `templates/hub.html` is the landing page template, and `circuits/<slug>/` holds each circuit's data (racing line, corner facts, meta copy). `build.js` renders one static page per circuit into `dist/<slug>/`, plus the hub into `dist/index.html`.

| Path | Description |
|------|-------------|
| `templates/circuit.html` | Shared HTML/CSS/JS template for every circuit page |
| `templates/hub.html` | Landing page template — circuit picker with a world map |
| `circuits/<slug>/data.js` | Racing line, corners, and facts for that circuit |
| `circuits/<slug>/meta.json` | Titles, descriptions, and other per-circuit copy |
| `circuits/<slug>/seo.html` | Hidden crawlable content block for that circuit |
| `build.js` | Renders `templates/` + `circuits/` into `dist/` (no dependencies — plain Node) |
| `dist/` | Build output — generated, not committed |
| `manifest.json` | PWA manifest (name, icons, display mode) |
| `sw.js` | Service worker — caches the app shell and OSM map tiles |
| `tools/align.html` | Dev-only tool for aligning a circuit's racing line and corner positions against real GPS data |
| `.github/workflows/deploy.yaml` | GitHub Actions CI — builds and deploys to Cloudflare Pages on push to main |

To work on a circuit's content or copy, edit its files under `circuits/<slug>/`, or `templates/circuit.html` / `templates/hub.html` for changes shared across pages, then run `node build.js` and open `dist/<slug>/index.html` (or `dist/index.html` for the hub) to preview.

## Adding a circuit

1. Create `circuits/<slug>/` with `data.js`, `meta.json`, and `seo.html`, following an existing circuit as a reference.
2. Use `tools/align.html` to align the racing line and corner positions against real map data.
3. Run `node build.js` and confirm the new page builds and the hub picks it up automatically.

## Deployment

Deployments are handled automatically via GitHub Actions on every push to `main`, which runs `node build.js` and publishes `dist/`. Two secrets are required in the repo settings:

- `CLOUDFLARE_API_TOKEN` — create at Cloudflare dashboard → My Profile → API Tokens
- `CLOUDFLARE_ACCOUNT_ID` — found on the Cloudflare Workers & Pages overview page

To deploy manually:

```bash
node build.js && npx wrangler pages deploy dist --project-name monaco-f1-walk
```

## Data & attribution

Racing line GPS from **[bacinger/f1-circuits](https://github.com/bacinger/f1-circuits)** (MIT licence, OpenStreetMap source).
Map tiles © [OpenStreetMap contributors](https://www.openstreetmap.org/copyright).
Historical facts are original writing.

## Issues & feedback

Found a corner that's off? A fact that's wrong? [Open an issue](https://github.com/drumandbytes/f1-walk/issues) — contributions and corrections welcome.

## Disclaimer

Unofficial fan project. Not affiliated with Formula One Licensing B.V., the Fédération Internationale de l'Automobile, or any circuit's organising body. "Formula 1", "F1", and related marks are trademarks of Formula One Licensing B.V.

## License

MIT — see [LICENSE](LICENSE). Third-party attributions and the fan-project
disclaimer are collected in [NOTICE](NOTICE).

---

Made by [Maris](https://drumandbytes.com) · [Buy me a coffee](https://buymeacoffee.com/justmaris)
