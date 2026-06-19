# 🧠 StudyAI Pro — Advanced Study Planner

> A fully client-side, AI-powered study planner that generates personalized roadmaps, tracks granular task completion, runs a Pomodoro focus timer, and lets students chat with an AI tutor — all without a backend.

![StudyAI Pro Banner](https://via.placeholder.com/900x280?text=StudyAI+Pro+—+Add+a+Screenshot+or+GIF+Here)

[![HTML5](https://img.shields.io/badge/Built%20with-HTML5-E34F26?style=flat&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-CSS-38BDF8?style=flat&logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![OpenAI GPT-4o](https://img.shields.io/badge/AI-GPT--4o--mini-412991?style=flat&logo=openai&logoColor=white)](https://platform.openai.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-22C55E?style=flat)](LICENSE)
[![Live Demo](https://img.shields.io/badge/Live-Demo-F97316?style=flat)](https://your-demo-link.vercel.app)

---

## The Problem

Students struggle to manage multiple subjects with different difficulty levels. They either burn time on easy topics or avoid hard ones altogether — and when they get stuck, they switch between 5 different apps to get help.

**StudyAI Pro** solves this in one zero-install HTML file:
- Builds a smart daily schedule weighted by difficulty and deadline
- Breaks every topic into specific, checkable sub-tasks
- Keeps you focused with a built-in Pomodoro timer
- Answers questions in context through an AI study buddy

---

## Live Demo

🔗 [Try it live →](https://your-demo-link.vercel.app)

> Open `index.html` directly in your browser — no server, no npm, no setup.

---

## Screenshots

| Setup screen | AI Roadmap | Focus timer | AI Chat |
|---|---|---|---|
| ![Setup](https://via.placeholder.com/200x130?text=Setup) | ![Roadmap](https://via.placeholder.com/200x130?text=Roadmap) | ![Timer](https://via.placeholder.com/200x130?text=Timer) | ![Chat](https://via.placeholder.com/200x130?text=Chat) |

> 💡 Record a short GIF with [ScreenToGif](https://www.screentogif.com/) and drop it here — it's the single highest-impact thing you can do for a README.

---

## Features

### Subject & schedule setup
- Add unlimited subjects with comma-separated sub-topics
- Tag each subject as Easy / Medium / Hard
- Set your exam date and daily study hours via a slider (1–14 h/day)

### AI-generated roadmap (`gpt-4o-mini`)
- Sends your full syllabus to the OpenAI API and receives a structured JSON plan
- Enforces a strict schema — each day includes: date, focus theme, priority level, time blocks, action type, recommended resources, and granular sub-tasks
- Renders a visual vertical timeline with day markers, connectors, and a milestone sidebar

### Sub-task completion tracking
- Every sub-task is individually checkable; state persists across tab switches in the same session
- Overall completion % auto-calculates across all days × blocks × sub-tasks
- Checked items show strikethrough; the progress bar on the Dashboard updates live

### Pomodoro focus timer
- Default 25-min work / 5-min break cycles
- Fully configurable via number inputs without stopping the current session
- Auto-switches between Work and Break with a toast notification on completion
- Reset button restores the current session type to its full duration

### Performance dashboard
- Shows overall completion %, daily target hours, number of subjects tracked
- Difficulty-weighted mastery bar rendered for each subject

### AI study buddy (chat)
- Maintains full conversation history per session
- Sends subject context alongside every message so answers stay curriculum-relevant
- Shows animated typing indicator while waiting for a response
- Enter key or send button submits; input is disabled during loading to prevent duplicate messages

### UX details
- Light / dark mode toggle with `localStorage` persistence
- Fully responsive: desktop nav bar + mobile bottom tab bar
- Smooth `fadeIn` animation on every view transition
- Retry logic with exponential backoff on API failures (up to 5 attempts)
- `escapeHTML()` sanitizer on all user-generated content rendered into the DOM

---

## Tech Stack

| Layer | Technology |
|---|---|
| Markup & structure | HTML5 (single file, no build step) |
| Styling | Tailwind CSS (CDN, dark mode via class strategy) |
| Icons | Lucide Icons (CDN) |
| Font | Plus Jakarta Sans via Google Fonts |
| AI — plan generation | OpenAI `gpt-4o-mini` with `response_format: json_object` |
| AI — chat | OpenAI `gpt-4o-mini` (conversational, plain text response) |
| State management | Vanilla JS module-level `state` object + `render()` |
| Persistence | `localStorage` (theme preference only) |
| Deployment | Any static host — Vercel, Netlify, GitHub Pages |

---

## Getting Started

### Option 1 — Open the file directly (no setup at all)

```bash
git clone https://github.com/your-username/studyai-pro.git
cd studyai-pro
# Open index.html in any modern browser
```

### Option 2 — Deploy to Vercel (under 60 seconds)

```bash
npm i -g vercel
vercel --prod
```

### Add your OpenAI API key

Open `index.html` and find this line near the top of the `<script>` tag:

```js
const apiKey = "sk-...your-key-here...";
```

Replace it with your key from [platform.openai.com/api-keys](https://platform.openai.com/api-keys).

> ⚠️ **Security note:** API keys in client-side JS are visible to anyone who inspects the page. See the [Security Notice](#security-notice) section below before deploying publicly.

---

## How It Works

```
User adds subjects (name + topics + difficulty) + exam date + daily hours
                          ↓
[Generate Plan] sends structured prompt + schema to GPT-4o-mini
                          ↓
API returns strict JSON: days → time blocks → sub-tasks
                          ↓
App renders vertical timeline roadmap with milestone sidebar
                          ↓
Student checks off sub-tasks → completion % updates in real time
                          ↓
Stuck on a concept? Open AI Chat → GPT answers with full subject context
                          ↓
Use Pomodoro timer to stay in deep work mode between sessions
```

---

## Project Structure

```
studyai-pro/
└── index.html
    ├── <head>              Tailwind CDN, Lucide CDN, Google Font, dark mode config
    ├── <header>            Desktop nav, theme toggle, reset button
    ├── <main #app-container>  View content is injected here on each render()
    ├── <nav> (mobile)      Bottom tab bar for small screens
    ├── #loading-overlay    Full-screen spinner shown during plan generation
    └── <script>
        ├── state {}                 Single source of truth for entire app
        ├── applyTheme()             Toggles dark class on <html>
        ├── setView()                Client-side router
        ├── callOpenAI()             API wrapper — handles JSON mode + retry
        ├── generatePlan()           Builds prompt + schema, calls API, sets state
        ├── addSubject()             Appends to state.subjects
        ├── toggleTimer()            Start / pause Pomodoro interval
        ├── handleTimerEnd()         Switches Work ↔ Break, shows toast
        ├── toggleTask()             Checks/unchecks a sub-task by key
        ├── calculateCompletion()    Counts done/total sub-tasks → %
        ├── handleSendChat()         Sends message + history to API
        ├── render()                 Full DOM re-render on every state change
        └── renderTimerDisplay()     Lightweight tick update (skips full render)
```

---

## OpenAI Response Schema
Plan generation uses `response_format: json_object` with this enforced structure:

```json
{
  "summary": "string",
  "days": [
    {
      "day": 1,
      "date": "YYYY-MM-DD",
      "focus": "string",
      "priority": "High | Medium | Low",
      "dailyGoal": "string",
      "blocks": [
        {
          "time": "string",
          "topic": "string",
          "action": "string",
          "resources": "string",
          "subTasks": ["string"]
        }
      ]
    }
  ],
  "milestones": [{ "title": "string", "description": "string" }],
  "globalTips": ["string"]
}
```

Strict schema enforcement prevents malformed JSON from breaking the UI — a common failure mode when using AI-generated structured data.

---

## Roadmap

- [x] AI roadmap generation with enforced JSON schema
- [x] Granular sub-task tracking with live completion %
- [x] Pomodoro timer with configurable work/break durations
- [x] Contextual AI chat with full conversation history
- [x] Light/dark mode with localStorage persistence
- [x] Exponential backoff retry on API failures
- [ ] Secure API key proxy via serverless function
- [ ] Export study plan as PDF
- [ ] Spaced repetition reminders via push notification
- [ ] Google Calendar sync for exam dates
- [ ] Offline support (PWA + IndexedDB cached plan)
- [ ] React Native / PWA mobile wrapper

## What I Learned

- How to use OpenAI's `response_format: json_object` and enforce a schema through the system prompt to guarantee parseable, UI-safe output — and why this matters more than hoping the model formats correctly
- How to architect a complete SPA in vanilla JS using a single `state` object and a `render()` function, without any framework
- Why `escapeHTML()` is non-negotiable when rendering user input into `innerHTML` — and how to implement it properly
- How exponential backoff retry logic works and why it matters for unreliable external APIs
- The real security trade-off between client-side API calls (simple, no backend) and server-side proxying (secure, needs a function) — and when each is appropriate for a student project

---

## Security Notice

The API key in this project is embedded in client-side JavaScript and **visible to anyone who views the page source**. This is fine for local use and demos, but not for a public URL.

For a secure public deployment:

1. Create a serverless function on Vercel, Netlify, or Cloudflare Workers
2. Move the OpenAI `fetch` call into that function
3. Store your API key as a server-side environment variable
4. Call your own `/api/chat` endpoint from the frontend instead of OpenAI directly

## Author

**Deekshitha D**
- GitHub: [@your-username](https://github.com/your-username)
- LinkedIn: [linkedin.com/in/your-profile](https://linkedin.com/in/your-profile)

