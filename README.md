# WC Air Miles ✈️🏆

Interactive map visualising every team's flight distance across all 23 World Cups (1930–2026).

**Live demo:** `http://localhost:8001/`

### Quick Start

```bash
python3 -m http.server 8001
# Open http://localhost:8001/
```

Zero dependencies. OpenStreetMap + Leaflet. No API keys.

### Data

Compiled from stat_man's per-tournament CSV research. 537 team entries, 369 KB.

### Built With

- Leaflet + OpenStreetMap
- Dark-first glassmorphism design
- Curved arc flight path rendering

### See Also

- `DESIGN.md` — full design spec
- `data/airmiles.json` — the unified dataset
