# Ottam — Fitness app

A single-page fitness app: daily workout plans (home + gym, rotating weekly),
a weekly-rotating affordable diet plan, live workout tracking, automatic
step/calorie detection on mobile, progress photos, and simple email/phone +
password login — all in one self-contained `index.html`.

There is **no backend server**. All data (accounts, profile, steps,
workouts, photos) is stored in the browser's `localStorage`, scoped to
whichever device/browser the person uses. Passwords are hashed
(SHA-256) before being stored, but this is not a substitute for a real
authentication backend — treat it as a demo/local-first auth flow.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire app — HTML, CSS, and JavaScript in one file |
| `manifest.json` | Lets phones install the app with its name/icon/colors |
| `sw.js` | Service worker — caches the app so it loads offline/instantly |
| `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | App icons |
| `LICENSE` | MIT license |

## Run it locally

No build step, no dependencies. Just serve the folder over HTTP(S) —
opening `index.html` directly via `file://` will work for most features,
but the service worker, camera capture, and motion-sensor step tracking
all require a real `http://` or `https://` origin.

```bash
# any static file server works, e.g.:
npx serve .
# or
python3 -m http.server 8080
```

Then open the printed local URL on your phone (same Wi-Fi network) or in
your desktop browser.

## Deploy for free with GitHub Pages

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under "Build and deployment", set **Source** to `Deploy from a branch`,
   pick your default branch and the `/ (root)` folder.
4. Save. GitHub gives you a URL like
   `https://<your-username>.github.io/<repo-name>/`.
5. Open that link on your phone → **Add to Home Screen** (iOS Safari:
   Share → Add to Home Screen; Android Chrome: ⋮ menu → Install app).

That's a fully working installable app icon, no app store needed.

## Turning it into an APK (optional)

Once it's hosted on a real `https://` URL (GitHub Pages works fine for
this), you can generate an installable Android package for free at
[pwabuilder.com](https://www.pwabuilder.com) — paste in your GitHub Pages
URL, click "Package for stores" → Android, and download the APK/AAB.
PWA Builder reads `manifest.json` and `sw.js` automatically.

## Known limitations (by design, not bugs)

- **No cross-device sync.** Accounts and data live in that browser's
  local storage only.
- **Automatic step tracking only works while the app is open on
  screen.** Mobile browsers suspend JavaScript (and sensor access) once
  the screen locks or the app is backgrounded, so true background step
  counting would need a native mobile app, not a web page.
- **Diet and workout plans are general suggestions**, not medical or
  dietetic advice.

## Editing

Everything lives in `index.html` — CSS is in the `<style>` block, JS in
the `<script>` block near the bottom. Search for these section markers
to find your way around:

- `AUTH / ACCOUNTS` — signup/login/logout and local account storage
- `DASHBOARD` — step/calorie rings
- `AUTOMATIC STEP TRACKING` — device-motion step + calorie detection
- `PROGRESS PHOTOS` — camera capture and photo grid
- `DIET` — the food database and weekly rotating meal plans
- `WORKOUT PLAN` — the weekly rotating body-part-focused workout plans
- `WORKOUT TRACKING` — the live activity timer (walking/running/gym/cycling)
- `PROFILE` — user detail form and BMI

No frameworks, no build tools — plain HTML/CSS/JS.
