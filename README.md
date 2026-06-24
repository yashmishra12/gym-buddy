# REP·BOARD

A single-file workout logging app. All your data lives in **your own private GitHub repo** — nothing goes through anyone else's server.

**Live app:** https://yashmishra12.github.io/gym-buddy/

## How it works

- The whole app is one self-contained `index.html`.
- Workout data is kept in the browser's `localStorage` and synced to a `data.json` file at the root of a private GitHub repo you control.
- The sync uses the GitHub Contents API directly from the browser. There is no backend.
- Your personal access token is stored only in your browser. The app strips it out of anything it writes to the repo.

## Setup

1. **Create a private repo** to hold your logs. (I use [`gym-log`](https://github.com/yashmishra12/gym-log) — set to **private**, Pages **off**.) The repo just needs to exist with a default branch; `data.json` is created on the first sync.

2. **Generate a fine-grained personal access token** at GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens:
   - Resource owner: your account
   - Repository access: *Only select repositories* → your data repo
   - Permissions → Repository → **Contents: Read and write** (Metadata: Read-only is added automatically)
   - Pick an expiration and click Generate. Copy the token once.

3. **Open the app** on whatever device you want to log from. On mobile, Add to Home Screen for an app-like feel.

4. In the app, go to **Settings → GitHub Sync**:
   - Repo: `your-username/your-data-repo`
   - Token: paste it
   - Tap Sync. Status should read `Synced ✓`.

5. Log a workout. It auto-syncs.

## Privacy

- This repo (`gym-buddy`) is public — it only contains the app code, no data.
- All your workout data lives in your own private repo. Never make that one public, and never enable Pages on it.
- The token is held in browser `localStorage`. Anyone with physical access to an unlocked device with the app installed can read it. Use a token expiry that matches your comfort level, and revoke it from GitHub if a device is lost.
- The app does not phone home. The only network calls it makes are to `api.github.com`.

## Tech

- Vanilla HTML/JS/CSS in one file. No build step, no dependencies.
- Hosted via GitHub Pages straight from `main`.

## Troubleshooting

- **"Failed to fetch"** — you're opening the app inside a sandboxed in-app browser (e.g. a webview embedded in another app). Open the real `https://yashmishra12.github.io/gym-buddy/` URL in Safari or Chrome.
- **"Auth failed"** — token expired, isn't scoped to the data repo, or is missing Contents: Read and write. Reissue it.
- **Pages 404 right after enabling** — the build hasn't finished. Wait ~1 minute.
