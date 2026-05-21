# FitnessMe

A personal fitness dashboard that asks Claude AI to generate a 7-day plan tailored to your birthday, gender, and end goal — then lets you iterate week by week.

## How it works

1. **Onboard** — enter your birthday, gender, end goal, and Anthropic API key.
2. **AI builds your plan** — Claude generates a 7-day plan with structured exercises (sets/reps/weight for strength, duration/distance/speed/incline for cardio, etc.).
3. **Log what you actually did** — check off exercises, delete ones you didn't do, add custom exercises (e.g. "treadmill at 5° incline, 5km/h, 20 min").
4. **Write daily notes** — how are you feeling? energy? soreness?
5. **Consult Fitness** — tap anytime to send all your past data + notes to Claude. It generates your next 7-day plan based on what worked and how you've been feeling.

## Install on phone

1. Open the URL in **Safari** (iPhone) or **Chrome** (Android)
2. Tap Share → **Add to Home Screen**
3. Launches like a native app

## Local development

Single self-contained HTML file. Open `index.html` in any browser.

## Files

- `index.html` — the entire app
- `manifest.json` — PWA manifest
- `icon.png` — app icon
- `CONTEXT.md` — full project context (architecture, data model, AI integration)
- `DEPLOY.md` — deployment guide

## API key

You bring your own Anthropic API key (`sk-ant-...` from console.anthropic.com). It's stored locally in your browser — never sent anywhere except directly to Anthropic's API.

## License

Personal use.
