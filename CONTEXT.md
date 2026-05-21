# FitnessMe — AI 7-Day Fitness Planner

## Project Overview
A personal fitness dashboard that generates a tailored 7-day plan via Claude AI based on user profile (birthday, gender, end goal). The user logs what they actually do each day, adds custom exercises (gym equipment, cardio with structured parameters), and writes daily notes. A "Consult Fitness" button sends all of that back to Claude to generate the next 7-day cycle.

Single-file HTML web app with mobile-first PWA support.

## Core Flow

1. **Onboarding** — modal asks for birthday, gender, end goal (free text), and Anthropic API key. Claude generates the first 7-day plan.
2. **Today / Plan** — current day shows AI-generated exercises with structured parameters. User can:
   - Check off exercises as done
   - Delete exercises they don't want
   - Add custom exercises with structured fields
   - Write a daily note ("how are you feeling")
3. **Consult Fitness** — persistent floating button. Tap to send the current cycle (planned + completed + custom + removed + notes) to Claude. A new 7-day plan is generated based on that data; the old cycle is archived to History.

## Data Model (localStorage key: `fitnessme_v6`)

```js
State = {
  profile: { birthday, gender, goal, createdAt },
  apiKey: '',
  model: 'claude-haiku-4-5-20251001',
  cycles: [
    {
      id, startDate, generatedAt, generatedBy: 'initial' | 'consult',
      summary,
      days: [
        {
          date,            // ISO YYYY-MM-DD
          title,           // e.g. "Upper Push", AI-provided
          summary,
          exercises: [{
            id, name,
            category: 'strength' | 'cardio' | 'mobility' | 'rest',
            sets, reps, weight,
            duration_min, distance_km, speed_kmh, incline_deg, rest_sec,
            notes,
            source: 'ai' | 'custom',
            done: bool
          }],
          note: '',         // user's daily feelings
          removed: [{name, category}]  // exercises the user deleted (sent on consult)
        }
      ]
    }
  ],
  activeCycleId,
  activeDay,                // ISO date currently focused in Plan pane
  archived: [...past cycles]
}
```

## UI

- **Today** — current day of the active cycle. Empty state if no plan exists yet.
- **Plan** — 7-day strip + selected day's full detail (edit any day in the cycle).
- **History** — list of past archived cycles, tap to view full detail.
- **Settings** — edit profile, manage API key/model, export/import JSON, reset.

## Claude Integration (BYOK)

Two system prompts in `index.html`:

- `SYSTEM_PROMPT_INITIAL` — generates the very first plan from profile.
- `SYSTEM_PROMPT_CONSULT` — generates the next plan from prior cycle data + user message.

Both return strict JSON with this shape:
```json
{
  "summary": "...",
  "days": [
    {
      "title": "Upper Push",
      "summary": "...",
      "exercises": [
        { "name": "...", "category": "strength|cardio|mobility|rest",
          "sets": 3, "reps": "8-12", "weight": 20,
          "duration_min": null, "distance_km": null, "speed_kmh": null,
          "incline_deg": null, "rest_sec": 90, "notes": "..." }
      ]
    }
  ]
}
```

API call uses `anthropic-dangerous-direct-browser-access: true` header. API key stored in localStorage.

## Add Custom Exercise

Modal with name + category radio + conditional structured fields:
- **Strength** — sets, reps, weight (kg), rest (sec)
- **Cardio** — duration (min), distance (km), speed (km/h), incline (°)
- **Mobility** — duration (min), sets

Plus optional notes. Custom exercises are tagged `source: 'custom'` and rendered with a "custom" badge.

## Files
- `index.html` — entire app (~1000 lines, single self-contained file)
- `manifest.json` — PWA manifest
- `icon.png` — app icon
- `CONTEXT.md` — this file
- `DEPLOY.md` — deployment guide
- `README.md` — public-facing overview

## Key Decisions
- Single HTML file with vanilla JS — no build, no server, works offline (except the AI calls)
- localStorage only — no backend
- Mobile-first, bottom nav
- 7-day cycles instead of fixed 30-day — designed for iterative AI-driven adjustment
- Profile is AI-driving (age + gender + free-text goal) — no hardcoded constraints; user describes their own limits and the AI programs around them
- Custom exercises are first-class: structured fields so cardio with speed/incline/distance works naturally
- "Consult Fitness" is always available — user decides when to regenerate; not gated to day 7
- Archived cycles preserve full data (planned + done + notes) for future reference
