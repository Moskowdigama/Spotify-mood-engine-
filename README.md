# 🎧 MoodMix

**A mood-based music recommendation engine that connects to your real YouTube account.**

Pick how you're feeling — or drop a combo of emojis — and get real songs that match. Connect your YouTube account and see a breakdown of the moods behind your actual liked videos.

🔗 **Live app:** [whatever-u-say-beautiful.streamlit.app](https://whatever-u-say-beautiful.streamlit.app)

---

## What this is

This is my second machine learning project, and my first time connecting a deployed app to a real third-party API (YouTube Data API v3) with full OAuth login — built and shipped entirely from my phone. No laptop, no PC. Kaggle for the model, GitHub mobile for the code, Streamlit Cloud for deployment.

It's part of a personal 30-day challenge: one project or meaningful improvement, every day.

---

## How it works

1. **Data & mood classification** — Built on a Kaggle dataset of 10,400 songs spanning 10 genres (1923–2023), with real audio features: `valence`, `energy`, `danceability`, `tempo`, `acousticness`, and more.

   Each song gets classified into one of five moods based on its **valence** (musical positivity) and **energy**:
   - 🔥 Happy/Hype
   - 😌 Chill/Content
   - 🎯 Neutral/Focused
   - ⚡ Angry/Intense
   - 🌧️ Sad/Melancholic

2. **Recommendation engine** — Pick a mood (or type any emoji combo — the app interprets it using the `emoji` library, not a hardcoded list) and optionally filter by genre. Songs are ranked by mood match and popularity.

3. **YouTube integration** — Connect your real YouTube account via Google OAuth. The app pulls your liked videos, fuzzy-matches each title against the song dataset (`difflib.get_close_matches`), and shows a bar chart breakdown of the moods behind what you actually watch.

---

## Tech stack

- **Model/data prep:** Python, pandas, Kaggle notebooks
- **App/UI:** Streamlit
- **Auth:** Google OAuth 2.0 (manual token exchange via `requests` — bypassed `google-auth-oauthlib`'s `Flow` object due to persistent PKCE/session-state issues inside Streamlit's rerun model)
- **API:** YouTube Data API v3
- **Hosting:** Streamlit Community Cloud, deployed straight from GitHub

---

## Known limitations (honest list)

- **No login persistence** — Streamlit's `session_state` resets on every fresh visit, so you have to reconnect YouTube each time. Fixing this needs a real backend (Firebase/Supabase) — planned but not built yet.
- **UI is functional, not final** — styled after Spotify's design language (dark, functional green, pill shapes) but still rough in places, especially the mood breakdown chart and song list cards.
- **Fuzzy matching has limits** — if a liked video's title doesn't reasonably resemble a song in the dataset, it won't be matched. No LLM fallback yet for unmatched titles.
- **No badges/gamification yet** — planned but not built.

---

## Planned next (when I pick this back up)

1. **Firebase integration** — persistent login, so users don't reconnect every session
2. **Badges system** — earn badges based on mood history (e.g. "Chill Master" for 60%+ Chill/Content listening)
3. **Persistent mood history** — track mood trends over time, not just per-session
4. **LLM-assisted matching** — use a free LLM API (Groq/Gemini) to clean messy YouTube titles and infer mood for songs that don't match the dataset

---

## Why I built this

I wanted something that combined an actual ML model with a real, working app feature — not just a notebook that outputs numbers. Getting OAuth working on a phone, with no local dev environment, debugging PKCE errors through screenshots and GitHub's mobile editor, was genuinely the hardest part of this project. It broke a bunch of times. It works now.

Built entirely on a phone. No PC. No Termux. Just Kaggle, GitHub, Streamlit, and a lot of patience.

---

## Credits

Dataset: [10400 Classic Hits - 10 Genres - 1923 to 2023](https://www.kaggle.com/datasets/thebumpkin/10400-classic-hits-10-genres-1923-to-2023) (Kaggle)
