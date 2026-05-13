# Deploy FitnessMe to your phone

You have three options, ordered by polish.

---

## Option 1 (recommended) — GitHub Pages, install as an app

You get a permanent URL like `https://yongjoo.github.io/fitnessme/` that works on every device, and you can "Add to Home Screen" on iPhone to make it look like a native app.

### Steps

1. **Create a GitHub repo**
   - Go to https://github.com/new
   - Name: `fitnessme` (or anything you like)
   - Public (required for free GitHub Pages)
   - Click Create

2. **Upload the files**

   Easiest way — use the web upload:
   - In the new repo, click **Add file → Upload files**
   - Drag in: `index.html`, `manifest.json`, `icon.png`
   - Commit

   Or use git from terminal:
   ```bash
   cd "/Users/sohyongjoo/Desktop/FitnessMe/Fitness Me"
   git init
   git add index.html manifest.json icon.png
   git commit -m "Initial fitness dashboard"
   git branch -M main
   git remote add origin https://github.com/<your-username>/fitnessme.git
   git push -u origin main
   ```

3. **Enable GitHub Pages**
   - In the repo: **Settings → Pages**
   - Source: **Deploy from a branch**
   - Branch: **main**, folder: **/ (root)**
   - Save. Wait ~1 minute.
   - Your URL appears at the top of that page, like `https://<your-username>.github.io/fitnessme/`

4. **Install on iPhone**
   - Open the URL in **Safari** (not Chrome — iOS only installs PWAs from Safari)
   - Tap the **Share button** (square with arrow up)
   - Scroll down → **Add to Home Screen**
   - The FitnessMe icon will appear on your home screen and launch full-screen, no browser bar

5. **Install on Android**
   - Open the URL in **Chrome**
   - Chrome will show "Install app" prompt — tap it
   - Or: menu → **Add to Home screen**

---

## Option 2 — AirDrop the HTML file (no hosting)

Quickest, no setup, but no permanent URL.

1. Right-click `index.html` → Share → AirDrop to your iPhone
2. On iPhone, when it asks where to open, choose Safari
3. Safari opens it from local file. Add to Home Screen still works.

Limitation: each device has its own data (no sync).

---

## Option 3 — Cloud sync (future)

The current build uses localStorage — data is stored per-device. If you want true cross-device sync (start a workout on laptop, finish on phone), let me know and I can add:
- Firebase / Supabase backend (free tier)
- Or sync via your iCloud Drive
- Or sync to a private GitHub Gist

---

## Data sync between devices (for now)

Use **Export data** on the Settings tab to save a JSON file, then import it on the other device (import feature coming in v2 — for now, paste into localStorage manually or just use one device).

The simplest workflow: **install on your phone only** and treat that as the source of truth. Use the laptop browser as a backup.
