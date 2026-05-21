# Steam Achievements Twitch Extension

A Twitch panel extension that displays a streamer's Steam game library and achievement progress in real time.

## What It Does

Viewers can open the panel to browse the streamer's Steam games. Clicking a game expands it to show all achievements — earned ones are displayed in full color, unearned ones are grayed out. Player avatar and name are shown in the header.

## Architecture

The UI is a React 18 single-page app built with Create React App. It communicates with a companion backend API (see [backend repo](#)) that proxies Steam Web API requests.

```
App
├── Header        ← player avatar + display name
└── Game[]        ← collapsible game cards
    └── Achievement[] ← lazy-loaded on expand
```

Data is fetched in two steps:
1. On load — player info and game list from `/api/simp/player?steamId=...`
2. On game expand — achievement list from `/api/simp/achievements?steamId=...&appId=...`

## Getting Started

### Prerequisites

- Node.js 16+
- The backend API running at `http://localhost:8080`

### Environment

Copy `.env` and set the Steam asset URLs:

```env
REACT_APP_STEAM_AVATAR_URL=https://avatars.akamai.steamstatic.com/
REACT_APP_STEAM_MEDIA_URL=http://media.steampowered.com/
```

### Run

```bash
npm install
npm start       # http://localhost:3000
```

### Build

```bash
npm run build   # production bundle → /build
```

Upload the `/build` folder to the Twitch Developer Console as the panel asset.

## Testing

```bash
npm test
```

Uses Jest and React Testing Library (via react-scripts).
