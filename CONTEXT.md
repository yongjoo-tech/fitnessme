# FitnessMe — 30-Day Fitness Dashboard

## Project Overview
A personal 30-day fitness plan dashboard for Yong Joo. Single-file HTML web app with mobile-first PWA support.

## Goals
- Lose 3kg by end of month
- Build V-shape physique (lats, shoulders, upper back focus)
- Increase overall strength
- Improve VO2 max

## User Profile
- Fitness level: Intermediate
- Gym access: Anytime Fitness + 5kg dumbbell at home
- Time per day: 30-60 minutes
- **Injury: Knee pain — likely Patellofemoral Pain Syndrome (PFPS)**

## Plan Constraints (PFPS-aware)
- No deep squats (limit to pain-free range, usually 0-60°)
- No lunges, jumping, or high-impact movements
- No running until knee settles
- Cardio = bike (low resistance), elliptical, incline walking
- Hip-hinge dominant lower body (RDLs over squats)
- Daily knee rehab routine (5 min, non-negotiable)

## Weekly Split
- Mon — Upper Push + LISS cardio
- Tue — Lower Hinge + Core
- Wed — Active Recovery + Knee rehab
- Thu — Upper Pull (V-shape focus)
- Fri — Full Body Strength + Core
- Sat — VO2 max intervals (4x4 Norwegian protocol on bike)
- Sun — Rest + mobility

## Adaptive Engine Logic
- Great / Good → Run plan as written (with encouragement)
- Okay → Plan as written, optional last-set drop
- Tired → Reduce sets by 1, lighter loads
- Sore → Swap to active recovery + mobility
- Knee pain → Auto-swap to upper body + low-resistance bike + extra rehab
- Sick → Full rest

## What Has Been Built (v4 — current)
v4 adds **real Claude AI coach** (BYOK — bring your own key):
- Settings → Fitness Coach card: paste Anthropic API key (`sk-ant-...`), pick model (Haiku 4.5 default, Sonnet 4.6, or Opus 4.6)
- "Test" button confirms the key works before relying on it
- When a key is set, `sendToCoach()` calls Anthropic Messages API directly from the browser using `anthropic-dangerous-direct-browser-access: true` header
- System prompt includes: PFPS constraint, full weekly split, exercise ID list, knee-rehab requirement, and structured JSON output schema
- User message includes: goal, current day, completion stats, mood counts, weight delta, last 7 days detail, last 3 conversations
- Claude returns JSON `{ response, adjustments[] }` — same shape as rules engine, so adjustments flow through identical `applyCoachAdjustments()`
- Each coach reply tagged with `source: 'claude' | 'rules'`, shown as 🧠 Claude or ⚙️ Rules in the chat
- Coach mode badge in Settings shows CLAUDE ON or RULES ENGINE
- API key stored masked (`••••••••XXXXXX`); cleared when user blanks the field
- **Graceful fallback** — if Claude API errors (invalid key, rate limit, network), it auto-falls back to the rules engine and toasts a warning
- Cost: ~$0.001 per message with Haiku, ~$0.01 with Sonnet

## What Has Been Built (v3)
v3 adds:
- **Goal banner** at top of Today screen — shows current goals ("V-shaped body · higher VO2 max · 3kg fat loss") with tap-to-edit
- **Consult Fitness Coach** floating button (bottom-right, above nav)
- **Coach modal** — full chat-style UI with:
  - Suggestion chips for first-time users ("My shoulder is sore", "I want more cardio", etc.)
  - Multi-line input with Enter-to-send, Shift+Enter for newline, auto-resize textarea
  - Typing indicator animation, smooth scroll-to-bottom
  - Conversation history persists across sessions
- **Smart Coach engine** (rules-based, no external API):
  - Reads last 7 days: completion rate, mood pattern, weight trend, knee pain count, tired count
  - Parses user message for: 8 pain types, fatigue, time, intent (more arms/core/cardio/V-shape/legs/strength/rest), progress signals
  - Generates concrete adjustments: avoid exercises, reduce volume, time-cap, add exercise, increase intensity
  - Adjustments apply to next 7 days automatically (3 days for fatigue)
  - Coach response includes empathetic open + data observations + applied changes
- **Coach adjustments** flow through `applyCoachAdjustments()` inside `getDayPlan()` — modifies upcoming exercises transparently
- **"Coach adjusted today" banner** on Today screen — shows when coach has modified the current day's workout
- **Settings → Fitness Coach card** — view active adjustments count + conversation count, clear either independently
- **Coach data** included in export/import JSON

## What Has Been Built (v2)
- `index.html` — Premium UX redesign:
  - **Mobile-first bottom navigation** (Today / Calendar / Progress / Plan / Settings)
  - **Hero card** with progress ring, streak counter, current weight
  - **Mood selector** with 7 options + adaptive plan changes shown inline
  - **Exercise cards** with collapsible form guides (target muscles + 5 form cues each)
  - **YouTube demo button** on every exercise (one-tap to video tutorial)
  - **Built-in rest timer** with quick 1:00 / 1:30 / 2:00 presets and vibration alert
  - **Streak tracking** (counts days with ≥80% completion)
  - **Quick weight log** on Today screen (in addition to Progress tab)
  - **Beautiful weight trend chart** with gradient fill, target line, daily deltas
  - **Per-type completion stats** (push/pull/hinge etc)
  - **30-day calendar** with status colors and tap-to-detail
  - **Toast notifications** for actions
  - **Onboarding modal** with smart defaults (auto target = start − 3kg)
  - **Data export + import** (JSON) for cross-device sync
  - **Celebration toast** when day 100% complete
  - **Smooth animations, dark theme, premium polish**
- `manifest.json` — PWA manifest for iPhone/Android home-screen install
- `icon.png` — 512×512 app icon
- `DEPLOY.md` — Step-by-step guide to host on GitHub Pages + install on phone

## Files
- `/CONTEXT.md` — Project context (this file)
- `/DEPLOY.md` — Phone-access deployment guide
- `/index.html` — The dashboard (open in any browser)
- `/manifest.json` — PWA manifest
- `/icon.png` — App icon

## How to Use
1. Open `index.html` in browser — onboarding modal pops up first time
2. Each morning, tap your mood → plan auto-adapts
3. Tap "📋 How to" on any exercise for form cues + muscle map
4. Tap "▶ Watch" to open YouTube demo
5. Tap the checkbox to mark complete (animates + toast)
6. Use ⏱ Rest timer between sets
7. Log weight on Today or Progress tab
8. Calendar tab → tap any day to see what's planned/done
9. Settings → export data regularly to back up

## Exercise Library
30+ exercises in `GUIDES` object, each with:
- Target muscles (chips)
- 5 specific form cues
- Curated YouTube search query (auto-opens search → user picks best video)

Covers all base exercises (push/pull/hinge/full/vo2/recovery/rest) + 5 knee rehab moves.

## Still To Do
- User to deploy to GitHub Pages → install as iPhone PWA (DEPLOY.md)
- (Optional) Real LLM-powered Coach via Anthropic API (user-provided key, browser direct mode)
- (Optional) Cloud sync via Firebase/Supabase for true cross-device data
- (Optional) Nutrition/calorie tracker (kcal in)
- (Optional) Per-set weight/rep logging for progressive overload tracking
- (Optional) Photo log (weekly progress pics)
- (Optional) Specific YouTube video IDs (curated) instead of search queries

## Key Decisions
- Single HTML file (1710 lines) with vanilla JS — no build, no server, works offline
- localStorage for persistence — simple, no backend needed
- Mobile-first responsive design with bottom nav (thumb-friendly)
- 30-day plan is generated from base templates + week progression + day-of-week slot
- Adaptive engine modifies exercises in real-time based on mood
- YouTube search URLs (no API key) for video demos — works forever, no auth
- Vibration API used on rest timer end (mobile haptic feedback)
- Auto target weight = start − 3kg in onboarding (smart default)
- Streak counted with ≥80% threshold (forgiving — life happens)
- Today is the hero — everything else is secondary navigation
- Hosting via GitHub Pages (free, permanent URL, matches user's GitHub workflow)
- Cross-device data sync via export/import JSON for v1
