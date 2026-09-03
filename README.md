# Bell Lap

*Vibe coded web app for customizable circuit training routines.*

A guided interval trainer. Runs a routine end to end: countdown per exercise,
animated figure showing the movement, rest between exercises and rounds, and
audio cues on the last three seconds plus start and stop.

Ships with three routines: **Strength Routine** (30 min, 3 rounds of a
six-exercise dumbbell circuit), **Classic 7-Minute Workout**, and a
**Custom Routine** slot that starts as a copy of the Strength Routine.
All three are editable in the app, and edits are saved on the device.

Everything is self-contained: no build step, no dependencies, no backend.

---

## Deploy to Netlify

**Drag and drop (fastest)**

1. Go to https://app.netlify.com/drop
2. Drag this whole folder onto the page.
3. Done. Netlify gives you a URL immediately.

**From a Git repo**

1. Push this folder to GitHub.
2. In Netlify: Add new site → Import an existing project → pick the repo.
3. Leave the build command blank. Set the publish directory to `/`
   (or to the folder name if you nested it).
4. Deploy.

## Deploy to GitHub Pages

This repo is https://github.com/OJ3PhD/bell-lap and publishes to
**https://oj3phd.github.io/bell-lap/**

1. Push to `main`.
2. Repo → Settings → Pages → Source: **Deploy from a branch**,
   Branch: `main`, Folder: `/ (root)`. Save.
3. After a minute the app is live at the URL above.

**The repository must be public** for Pages to publish on a free GitHub
account. Pages from a private repo requires a paid plan. Repo → Settings →
General → Danger Zone → Change visibility, if you need to switch it.

## The update loop

```
# edit index.html (or anything else)
git add -A
git commit -m "what changed"
git push
```

Pages redeploys within about a minute. If you changed app files, bump the
`CACHE` string in `sw.js` in the same commit so installed phones drop the old
cached copy and pick up the new build.

Every path in the app is relative, so it works from a subfolder like
`/bell-lap/` as well as from a domain root. No configuration needed either way.

**HTTPS is required** for the service worker and the screen wake lock.
Both GitHub Pages and Netlify serve HTTPS by default, so this is already handled.

---

## Install it on your phone

Open the deployed URL on your phone, then:

- **iPhone (Safari):** Share → Add to Home Screen.
- **Android (Chrome):** menu → Add to Home screen, or take the install prompt.

Because this build has a web app manifest and a service worker, it installs as
a real standalone app: its own icon, no browser chrome, and it runs offline
after the first load.

---

## Notes worth knowing

**Sound.** The tones are generated in the browser with the Web Audio API, so
there are no audio files to load. On iPhone, the physical silent switch mutes
them. Tap the speaker icon on the home screen to test before you start.

**Screen wake.** The app requests a screen wake lock while a workout is running
so your phone does not lock mid-set, and releases it when you pause or finish.
Supported in Chrome and in Safari 16.4 and later.

**Offline.** The service worker caches the app on first load. Fonts come from
Google Fonts and are cached the first time you are online; if they are ever
unavailable the app falls back to system fonts and stays fully usable.

**Your edits** are stored in `localStorage` on that device. They are not synced
between devices, and clearing site data resets the routines to their defaults.
"Reset to default" in the editor does the same for one routine.

**Timing** is based on wall-clock timestamps rather than counting ticks, so it
stays accurate if the browser throttles the page in the background.

---

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire app: markup, styles, and script in one file |
| `manifest.webmanifest` | App name, icons, standalone display mode |
| `sw.js` | Service worker: offline caching |
| `icon-192.png`, `icon-512.png` | App icons |
| `icon-maskable-512.png` | Android adaptive icon |
| `apple-touch-icon.png` | iOS home screen icon |
| `.nojekyll` | Stops GitHub Pages running the files through Jekyll |

## Updating after you change something

Edit `index.html`, then bump the `CACHE` version string at the top of `sw.js`
(`bell-lap-v1` → `bell-lap-v2`) and redeploy. That tells installed copies to
drop the old cache and pick up the new build.
