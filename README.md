# FitnessMe

Personal 30-day fitness dashboard with adaptive workouts, a built-in AI fitness coach, YouTube form-demo links, rest timer, weight tracking, streak counter, and PWA support for iPhone/Android home-screen install.

## Live

Once GitHub Pages is enabled: `https://<your-username>.github.io/fitnessme/`

## What it does

- 30-day plan tailored for V-shape, strength, VO2 max, and 3kg fat loss
- PFPS-safe (no deep squats, no impact, hip-hinge dominant lower body)
- Daily mood check that adapts the workout in real time
- Fitness Coach modal — type how you're feeling or what to change; it reads your history and adjusts the next 7 days
- Every exercise has form cues, target muscles, and a one-tap YouTube demo
- Built-in rest timer with vibration alert
- Beautiful weight trend chart, streak counter, completion stats by workout type
- Data persists in browser via localStorage · export/import JSON for backup

## Install on phone

1. Open the URL in **Safari** (iPhone) or **Chrome** (Android)
2. Tap Share → **Add to Home Screen**
3. Launches like a native app — full screen, no browser bar

## Local development

It's a single self-contained HTML file. Just open `index.html` in any browser.

## Files

- `index.html` — the entire app
- `manifest.json` — PWA manifest
- `icon.png` — app icon
- `CONTEXT.md` — full project context (plan structure, decisions, adaptive logic)
- `DEPLOY.md` — deployment guide

## License

Personal use.
