# Homework Log — PWA

A installable, offline-capable version of your Homework Log, ready to host on GitHub Pages.

## Files

```
index.html          ← your app, with PWA tags + service-worker registration added
manifest.json        ← app name, colours, icons (controls the "install app" prompt)
sw.js                 ← service worker (caches the app so it works offline)
icons/
  icon-192.png
  icon-512.png
  icon-maskable-512.png
  apple-touch-icon.png
```

All of your original data logic, styling and the WhatsApp export are untouched — this only
adds the files a browser needs to treat the page as an installable app.

## Deploy to GitHub Pages

1. Create a new repository on GitHub (public repos get free Pages hosting; private repos
   need GitHub Pro/Team/Enterprise for Pages).
2. Upload all the files above, **keeping the folder structure** — `icons/` must stay a
   subfolder next to `index.html`.
   - Easiest: on the repo page, click **Add file → Upload files**, drag in `index.html`,
     `manifest.json`, `sw.js`, and the `icons` folder (drag the whole folder — GitHub
     preserves the path), then commit.
3. Go to **Settings → Pages**.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**.
5. Pick branch `main` (or `master`) and folder `/ (root)`, then **Save**.
6. Wait ~1 minute, then your app will be live at:
   `https://<your-username>.github.io/<repo-name>/`

That URL is what you open on your phone and "Add to Home Screen" (iOS Safari) or
"Install app" (Android Chrome / desktop Chrome & Edge) — it'll behave like a native app,
open full-screen, and keep working without a connection.

## Notes on how it was made "adaptive"

- **Installable**: `manifest.json` gives it a name, icon and `"display": "standalone"`,
  so installing it hides the browser chrome and it opens like a real app.
- **Offline-capable**: `sw.js` caches the app shell on first visit. After that, opening
  the app works even with no signal — it just serves the cached version, and quietly
  refreshes the cache in the background whenever you're online.
- **Responsive**: the page already used relative units, `env(safe-area-inset-*)` for
  notches, and `prefers-color-scheme` for dark mode — that was kept as-is.
- **Data**: everything is still stored in the browser's `localStorage`, exactly as
  before. Installing as a PWA doesn't change that — it's still per-device, so the
  Export/Import backup buttons in Settings are still the way to move data between
  devices.

## If you rename the repo or move files

`manifest.json`'s `start_url`/`scope` and `sw.js`'s asset list use **relative** paths
(`./`, `icons/...`), so this works whether it's served from the repo root
(`username.github.io`) or a project path (`username.github.io/repo-name/`) — no edits
needed either way.

## Updating later

Whenever you upload a changed `index.html`, bump `CACHE_VERSION` at the top of `sw.js`
(e.g. `hwlog-v1` → `hwlog-v2`). That's what tells visitors' browsers to fetch the new
version instead of serving the old cached one.
