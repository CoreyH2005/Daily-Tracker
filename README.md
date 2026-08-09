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

## Sync

Data is written to `localStorage` first, then mirrored to a Firebase Realtime Database
(project `sitrep-sync`) under `/sitrep`. A poll every 15 seconds pulls changes back down,
so the phone and the PC stay in step. The app keeps working with no connection — writes
land locally and go out when the network returns. The footer shows sync state and this
device's id.

**What is shared with SITREP, and what isn't.** Only the AOSB and Sandhurst dates, at
`/sitrep/aosbDate` and `/sitrep/sandhurstDate`, stored as date-only ISO strings. Those are
one real-world fact, so they get one home. Everything else — schedule, habits, habit log,
templates, fitness — stays private to this app under `ops_`-prefixed keys, because SITREP
models the same ideas differently and merging them would corrupt one side or both.

## Data & privacy notes

- Your planner data leaves the device. It is held in a Firebase database, not just in the
  browser.
- Access is by anonymous auth plus a uid allowlist. **Anonymous sign-in mints a new uid per
  device, not per person** — and per browser profile, so SITREP's two kiosk windows count
  as two devices. Clearing a browser's storage gives that device a new uid.

### Adding a device to the allowlist

The rules in `firebase-rules.json` are generic — they check an allowlist held in the
database, so adding a device does not mean editing or republishing rules, and no uid is
ever committed to this repo.

1. Open the app on the new device and read the uid from the footer (SITREP shows its own
   in the top-right "DEVICE" readout).
2. Firebase Console → Realtime Database → **Data** tab → `allowedUids` → add a child named
   with that uid, value boolean `true`.

That is the whole procedure. `allowedUids` is unreadable and unwritable by any client; only
the rules engine can see it.
- Clearing site data still wipes local history, but anything already synced comes back on
  the next poll.

## Editing later

Colors, fonts and default habits live in `index.html`. The AOSB and Sandhurst dates are no
longer edited here — change them in the app (or in SITREP) and they propagate, since the
database is the source of truth. `DEFAULT_TARGETS` is only the fallback for a device that
has never synced.
