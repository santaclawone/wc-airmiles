# WC Air Miles — World Cup Flight Tracker

Visualising the distances every team has travelled across 23 World Cups (1930–2026).

## The Concept

A scrollable, interactive world map that shows **how far teams flew to every World Cup** — from Uruguay's 0 km in 1930 to Brazil's 223,908 km across 23 tournaments. Each World Cup is a snapshot. The timeline bar lets you scroll through all of them.

---

## Design Direction

_Based on the Empire Creative Director's design principles: dark-first, glassmorphism 2.0, motion as communication, Space Grotesk + Inter._

### Visual Style

- **Theme:** Dark immersive (`#0a0a0f` base, `#12121a` surfaces)
- **Glass elements:** Backdrop-blur cards with subtle border (`rgba(255,255,255,0.04)` fill, `rgba(255,255,255,0.08)` borders)
- **Accent colour:** Cyan (`#06b6d4`) — the "flight path" colour
- **Typography:** Space Grotesk (headings, bold confident) + Inter (body, clean readable)
- **Map tiles:** OpenStreetMap with dark filter (invert + hue-rotate + brightness 0.7)

### Layout

```
┌────────────────────────────────────────────────┐
│  ✈️ WC Air Miles     🌍 World │ 🏠 Country     │
├──────────────────────────────────────┬─────────┤
│                                      │  STATS  │
│        MAP (Leaflet + OSM)           │ SIDEBAR │
│   - World: curved arcs from capitals │ - This  │
│   - Country: zoomed to host,        │   WC    │
│     city markers + internal routes   │ - All-  │
│                                      │   Time  │
│                                      │ - Deep  │
│                                      │   Dive  │
├──────────────────────────────────────┴─────────┤
│  1930 │ 34 │ 38 │ 50 │ 54 │ … │ 22 │ 26         │
└────────────────────────────────────────────────┘
```

---

## Feature Spec

### 1. World View (default)

- **Great-circle arcs** from each team's capital city to their first host city
- Coloured by **continent** (Europe: blue, S. America: green, Africa: orange, Asia: red, N. America: yellow, Oceania: purple)
- Arc **thickness scales with distance** (thicker = further)
- **Hover** to highlight a team's route
- **Click** to open detailed team info in the sidebar

### 2. Host Country View

- Camera zooms into the host nation
- **Host city markers** with tooltips
- **Internal travel routes** drawn as dashed lines between host cities visited
- Teammate detail still accessible via sidebar

### 3. Timeline Bar

- All 23 World Cups as clickable chips at the bottom
- Shows year + host nation abbreviation
- Current selection highlighted in cyan
- Horizontally scrollable (overflow-x)
- Click any year to switch the map and stats

### 4. Stats Sidebar (3 tabs)

**This WC tab:**
- Mini stat cards: teams count, total km, avg km, furthest team
- Scrollable team list ranked by total km, with bar visualisation
- Click a team to show their route and stats

**All-Time tab:**
- Full leaderboard: every nation ranked by cumulative km across all World Cups
- Bar visualisation proportional to the leader
- Click any nation to see every World Cup they've played in

**Deep Dive tab:**
- **Marathon Men** — nations with the highest average km per appearance (Australia: 14,670 km avg across 7 tournaments)
- **Efficiency Kings** — most tournament progress per km travelled (Uruguay 1930: 0 km, champions)

### 5. Filter

- **QF+ toggle** — only show teams that reached the quarter-finals or better
- Great for focusing on the serious contenders vs. early-exit travellers

### 6. Info Cards (click any arc or team)

Shows:
- Route: Capital → First host city
- Flight km, Internal km, Total km
- Per-game average (when available)
- Games played
- Furthest stage reached
- Winner status

---

## Tech Stack

| Layer | Choice | Why |
|---|---|---|
| Map engine | Leaflet + OpenStreetMap | No API keys, no logins, free tiles |
| Map styling | CSS filter invert/hue-rotate | Dark theme without premium tiles |
| Arc rendering | L.polyline with calculated bezier points | Custom arc formula, no plugins needed |
| Data format | Single static JSON (369 KB) | Loads instantly, no backend |
| Hosting | Any static host | GitHub Pages, Vercel, Netlify — zero config |

### Data Architecture

`data/airmiles.json` structure:
```json
{
  "worldCups": [{
    "year": 1966,
    "host": "England",
    "hostLat": 52.36, "hostLng": -1.17,
    "hostCities": [{"name": "London", "lat": 51.51, "lng": -0.13}, ...],
    "teams": [{
      "nation": "England",
      "capital": "London",
      "capitalLat": 51.51, "capitalLng": -0.13,
      "firstHostCity": "London",
      "firstHostLat": 51.51, "firstHostLng": -0.13,
      "flightKm": 11.8, "internalKm": 0.0, "totalKm": 11.8,
      "kmPerGame": 2.0, "gamesPlayed": 6,
      "furthestStage": "Winner",
      "wonFinal": true,
      "notes": "Route: ..."
    }, ...]
  }, ...],
  "allTimeStats": {
    "leaderboard": [{"nation": "Brazil", "totalKm": 223908, ...}, ...],
    "longestTournament": {"year": 2026},
    "shortestTournament": {"year": 1934}
  }
}
```

---

## Edge Cases & Notes

- **Pre-1950 tournaments** had very few teams (13 in 1930) — the map looks sparse, but that's historically accurate
- **1934 Italy** — shortest tournament by total km (all European teams, compact geography)
- **2026** — longest tournament (48 teams across USA/Canada/Mexico, three host nations)
- **Teams with 0 internal km** (e.g. England 1966 playing every game at Wembley) — still show a minimal arc from capital to first host city
- **Australia** — the true marathon men: 7 appearances, 102,692 km (14,670 avg)
- **Dutch East Indies** (1938) — one appearance, one historical note
- **Soviet Union / West Germany / East Germany / Yugoslavia / Czechoslovakia** — defunct nations preserved as they appeared

---

## Future Enhancements

- [ ] **Split-screen compare mode** — view two World Cups side-by-side
- [ ] **Animated transitions** — arcs draw on sequentially when switching years
- [ ] **Continent filter** — "Show me what Asian teams travelled in 2006"
- [ ] **Detailed route viewer** — parse notes to show the exact venues on the map
- [ ] **Player-level data** — include player birthplaces for "Player Origin" view
- [ ] **Download as image** — share a WC snapshot
- [ ] **Light mode** — as an option, though dark is the default

---

## Files

```
wc-airmiles/
├── index.html           # Complete single-page application
├── data/
│   └── airmiles.json    # Unified World Cup flight data (369 KB)
├── DESIGN.md            # This document
└── README.md            # Quick start
```

No build tools, no dependencies, no API keys. Open `index.html` from any static server and it works.
