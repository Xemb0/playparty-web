# PlayParty — Web

Standalone marketing site for **PlayParty** (party games with friends over live video/voice), ready to deploy on Netlify. Mirrors the WatchParty site structure.

## Structure

- `index.html` — marketing landing page (Tailwind via CDN, no build step) + iOS-beta signup + screenshot gallery + room-invite overlay.
- `/app/` — the compiled Kotlin Multiplatform PlayParty web app (Compose/wasmJs, `playparty.js`). The "Web" badge / "Launch Web App" buttons open it. **Not yet generated — see below.**
- `privacy-policy.html`, `terms-of-service.html`, `contact.html` — legal/contact pages.
- `js/` — `supabase-config.js` (browser-public anon/publishable key) + `gallery.js`.
- `netlify.toml` — WASM mime type, caching for the wasm/js bundles, and the SPA fallback for the app's client-side router.
- `files/` — drop the release `playparty.apk` here (the "Download Android APK" button links to `files/playparty.apk`).

## Before going live (assets to add)
- `files/playparty.apk` — the signed release APK.
- Supabase `marketing/playparty/` bucket: `playparty_logo.png`, `feature_graphic.png`, `1.png`…`5.png` (screenshots). Missing screenshots are hidden gracefully.
- (Optional) build the web app into `/app/` — see below. Until then the Web buttons 404.

## Build the web app into /app/
```bash
cd ..   # PlayParty repo root
./gradlew :composeApp:wasmJsBrowserDistribution
cp -R composeApp/build/dist/wasmJs/productionExecutable/* playparty-web/app/
```
(See kmp-essentials §11 for the wasmJs Coil-image gotcha.)

## Deploy to Netlify (custom domain: playparty.space)
1. New site → Import from Git → pick this repo (or the `playparty-web` subdir).
2. Build command: *none*. Publish directory: `.` (set in `netlify.toml`).
3. Deploy. Landing serves at `/`, the app at `/app/`. Pushing to the default branch auto-deploys.

> The Supabase key in `js/supabase-config.js` is a public anon/publishable key meant to live in the browser. Row-level security protects data, not this key.
