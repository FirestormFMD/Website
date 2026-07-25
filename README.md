# FirestormFMD Website

Static site for FirestormFMD, deployable via GitHub Pages.

## Tracked in this repo

- `index.html` — the live site. Fully self-contained: fonts load from the Google Fonts CDN, everything else (script, styles, the logo image) is bundled inline. Site content (mods, modpacks, WIPs, updates, bugs) is fetched at runtime from `data/mods.json`.
- `data/mods.json` — all site content. Edited from the admin panel (or by hand).
- `files/` — uploaded mod jars, modpacks and preview builds, organised as `files/{mods,packs,wips}/<id>/<filename>`. Created by the admin panel.
- `admin.html` — owner-only content manager (see below).
- `LICENSE.txt` — GPL-3.0 license.
- `assets/fmd-logo.png` — source logo asset, kept for reference/future edits.

## Managing content (admin panel)

Open `admin.html` on the deployed site (or locally) and sign in with a GitHub
**fine-grained personal access token**:

1. GitHub → Settings → Developer settings → Fine-grained personal access tokens → *Generate new token*.
2. Repository access: **Only select repositories** → this repo.
3. Permissions → Repository permissions → **Contents: Read and write** (nothing else).

The panel commits straight to this repo through the GitHub API, so the token is
the only credential — nobody without it can change anything. What it can do:

- **Mods** — add/edit/remove mods, edit the featured release and its changelog +
  required-mods list, and **Publish release** (archives the current featured
  version, uploads the new `.jar`, optionally auto-posts to Updates).
- **Modpacks** — add/edit/remove, upload `.zip`/`.mrpack` files.
- **WIPs** — add/edit/remove work-in-progress entries (stage label, progress %,
  optional preview build download).
- **Updates** — news posts shown on the Updates page.
- **Bugs** — entries on the public bug tracker.
- **Site** — name/title/tagline, which Downloads tabs are shown, and the
  appearance a first-time visitor lands on: colour mode (dark / light / follow
  their device), accent hue, and which home layout to use (centred or
  dashboard). The site itself has a theme picker in the topbar; once a visitor
  chooses there it's remembered in their browser and these defaults no longer
  apply to them.
- **Raw JSON** — direct editor for `data/mods.json` for anything the forms
  don't cover (reordering, editing archived versions, …).

Notes:

- File uploads commit immediately; everything else commits when you hit
  **Save & publish**. GitHub Pages redeploys in about a minute.
- Uploads go through the GitHub contents API, which caps files at ~50 MB. For
  bigger files, host them elsewhere and paste the URL in the file widget.
- The token can be remembered in the browser's localStorage — only do that on
  your own machine.
- Saving from the admin panel commits to GitHub; if you also keep a local
  clone, `git pull` afterwards.

## Ignored (see `.gitignore`)

- `FirestormFMD Site.dc.html`, `FirestormFMD Explorations.dc.html`, `support.js` — source files for editing the site in the design canvas tool. They require a runtime (`support.js`) to render and aren't standalone pages, so they're kept locally but not pushed.
- `.thumbnail` — internal preview image from the design tool.
- `screenshots/`, `uploads/` — draft/duplicate images not referenced anywhere in `index.html`.
- `Firestorm FMD Site Redesign/` — the working folder the current design was drafted in. Its contents are merged into the site proper; the copies of `assets/` and `data/` it still holds are stale.

## Deploying

Push to GitHub, then in the repo go to Settings → Pages → set source to the default branch, root folder. The site will be served at your repo's Pages URL, rendering `index.html`.

