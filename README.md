# Daily Ops

A personal daily schedule + habit tracker, styled to match The Global Monitor. Runs entirely in the browser — no backend, no login, data saves to the device you're using via `localStorage`.

## What's in here

- `index.html` — the whole app (one file, no build step)
- `manifest.json` — lets you "Add to Home Screen" on iOS/Android so it opens full-screen like a native app
- `icon.svg`, `icon-192.png`, `icon-512.png` — app icons

## Deploy it (GitHub Pages, same pattern as The Global Monitor)

1. **Create a new repo** — e.g. `CoreyH2005/daily-ops` — and push these files to the root (or to a `docs/` folder if you'd rather keep `main` clean).

   ```bash
   git init
   git add .
   git commit -m "Daily Ops v1"
   git branch -M main
   git remote add origin https://github.com/CoreyH2005/daily-ops.git
   git push -u origin main
   ```

2. **Enable GitHub Pages** — repo → Settings → Pages → Source: deploy from `main` branch, root folder. Save. GitHub will give you a URL like `https://coreyh2005.github.io/daily-ops/`.

3. **Custom domain (optional)** — if you want something like `ops.yourdomain.com`:
   - In Namecheap: add a CNAME record — Host: `ops`, Value: `coreyh2005.github.io`
   - In the repo's Pages settings: enter the custom domain, tick "Enforce HTTPS" once the certificate provisions (can take a few minutes to an hour)

4. **Add to your phone's home screen** — open the deployed URL in Safari (iOS) or Chrome (Android), then "Add to Home Screen". It'll launch full-screen with no browser chrome, using the icon and name from `manifest.json`.

## Data & privacy notes

- Everything is stored in the browser's `localStorage` on whatever device you're using — nothing is sent anywhere, there's no server, no account.
- **This means data does not sync across devices.** If you use it on your phone and your laptop, they'll have separate histories. If that becomes a problem, the fix is adding a small backend (e.g. a free Supabase or Firebase project) to sync habit logs and schedule blocks — worth doing later if you actually want cross-device continuity, not worth the complexity for v1.
- Clearing your browser's site data/cache will wipe the planner. Don't do a "clear all browsing data" without exporting first (no export feature yet — flag it if you want one).

## Editing later

Everything — colors, fonts, default habits, the AOSB/Sandhurst target dates — lives in `index.html`. The AOSB date is hardcoded as `new Date(2026, 9, 12)` (Oct 12, 2026) and Sandhurst as `new Date(2027, 8, 1)` (Sep 2027, approximate) near the top of the `<script>` block — update those if dates shift.
