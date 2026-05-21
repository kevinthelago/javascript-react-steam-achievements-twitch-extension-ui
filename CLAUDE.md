# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm start        # Dev server at localhost:3000
npm run build    # Production build → /build
npm test         # Run tests (Jest/React Testing Library, watch mode)
```

Single test file: `npm test -- --testPathPattern=App`

## Architecture

This is a React 18 Twitch extension panel that shows a Steam player's game library and achievement progress. It talks to a separate backend API (expected at `http://localhost:8080/api/simp/`).

**Component tree:**
```
App (class component)
├── Header            ← player avatar + name
└── Game[]            ← one per game in player's library
    └── Achievement[] ← rendered after game is expanded
```

**Data flow:**
1. `App.jsx` fetches `/player?steamId=...` on mount → receives `{ avatarHash, steamName, games[] }`
2. Each `Game.jsx` fetches `/achievements?steamId=...&appId=...` lazily when the card is expanded
3. No global state — all state is local to class components via `this.state`

**Environment variables** (defined in `.env`, injected by CRA as `process.env.REACT_APP_*`):
- `REACT_APP_STEAM_AVATAR_URL` — base URL for Steam avatar images
- `REACT_APP_STEAM_MEDIA_URL` — base URL for Steam media assets

## Key Conventions

- All components use React class component syntax (not hooks)
- Styling is plain CSS with heavy use of CSS custom properties defined in `src/color.css`
- Root font-size is set to 62.5% so `1rem = 10px` throughout
- Twitch dark/light themes are handled via `.theme-dark` / `.theme-light` class selectors on the root
- The Twitch extension helper script is loaded in `public/index.html` but wiring to `window.Twitch.ext` is not yet implemented — the Steam ID is currently hardcoded in `App.jsx`
- `TwitchAchievementList.jsx` and `StreamAchievementList.jsx` are stubs — not imported or rendered anywhere
