# CJ's Task Tracker — installable app

This folder is a complete, self-contained web app (PWA). Once it's hosted on a real URL, you can install it like a native app on both your desktop and your Android phone — icon, full-screen, works offline, and your checked-off items are saved on that device.

## Publish it on GitHub Pages (no command line needed)

1. Go to [github.com](https://github.com) and sign in (or create a free account if you don't have one).
2. Click **New repository** (top right → the `+` icon). Name it something like `cj-task-tracker`, set it to **Public**, and click **Create repository**.
3. On the new repo's page, click **uploading an existing file**.
4. Drag in all the files from this folder — `index.html`, `manifest.json`, `service-worker.js`, and the whole `icons` folder — then click **Commit changes**.
5. Go to the repo's **Settings** tab → **Pages** (left sidebar) → under **Build and deployment**, set **Source** to "Deploy from a branch", **Branch** to `main` and folder `/ (root)`, then **Save**.
6. Wait about a minute, then refresh that Pages settings page — it'll show your live URL, something like:
   `https://<your-github-username>.github.io/cj-task-tracker/`

## Install it

- **Android (Chrome):** open that URL, tap the **⋮** menu → **Add to Home screen** / **Install app**. It'll behave like a normal app with its own icon.
- **Desktop (Chrome or Edge):** open the URL, click the install icon in the address bar (or use the **Install app** button in the top-right of the app itself), and it opens in its own window from then on.

## Cross-device sync (optional, free — Firebase)

Without this step, the app still works and saves your checked-off items — but only on the one device you're using, via browser local storage. To have your desktop and Android installs show the same progress, set up Firebase (Google's app backend). It's genuinely free for this — the Spark (free) plan covers Authentication and Firestore with quotas far beyond what a personal tracker needs, no credit card required.

**1. Create the project**
- Go to [console.firebase.google.com](https://console.firebase.google.com), sign in with your Google account, click **Add project**, name it (e.g. `cj-task-tracker`), and finish the wizard (you can skip Google Analytics — not needed here).

**2. Register a web app**
- On the project's home page, click the **`</>`** (web) icon to add a web app. Give it any nickname, click **Register app** — you don't need Firebase Hosting for this.
- It'll show you a `firebaseConfig` object (apiKey, authDomain, projectId, etc.). Copy those values into `firebase-config.js` in this folder, replacing the `REPLACE_ME` placeholders.

**3. Turn on sign-in**
- In the left sidebar: **Build → Authentication → Get started**. Under the **Sign-in method** tab, enable **Google**, pick a support email (your own), and save.

**4. Turn on the database**
- **Build → Firestore Database → Create database**. Choose any region close to you, and start in **production mode** (we're supplying our own rules, not the open test-mode ones).
- Once created, go to the **Rules** tab and replace the contents with what's in `firestore.rules` in this folder, then **Publish**. This locks the data so only you (signed in) can read or write your own tracker data — nobody else's.

**5. Re-upload to GitHub Pages**
- Upload the updated `firebase-config.js` (and `index.html`, `service-worker.js` if you re-pulled them from Claude) to your GitHub repo the same way as before — drag and drop, commit.

**6. Sign in on both devices**
- Open the app on your phone and desktop, click **Sign in with Google** on each (top right), use the same Google account both times. From then on, ticking something off on one device shows up on the other within a second or two — Firestore keeps them in sync in real time. If you're offline, changes are queued and sync automatically once you're back online.

If you skip this section entirely, the app defaults to local-only mode automatically — nothing breaks, it just won't sync across devices.

## Updating it later

When you want changes (new categories, tracker updates, etc.), tell Claude in Cowork — it'll regenerate `index.html` and hand you the updated file to re-upload to the same GitHub repo (drag-and-drop replace, same as step 4). Your checked-off progress lives in the browser's local storage on each device, separate from the file itself, so re-uploading a new version won't wipe it out unless the item list itself changes.

## Note on this being "for testing"

This is a PWA (installable web app), not a Play Store / App Store native build. It's the fastest real path to something installable on both your phone and desktop today. If down the line you want a real `.apk` for Play Store distribution or a packaged desktop installer, that's a separate, bigger build (via Capacitor for Android, Electron/Tauri for desktop) — ask and we can scope that out.
