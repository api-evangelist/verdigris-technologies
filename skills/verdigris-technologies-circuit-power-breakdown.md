---
name: Break down building power to circuits with Verdigris
description: Drill from panel-level to circuit-level average power to find where a building's load is concentrated.
api: openapi/verdigris-technologies-data-v4-openapi.json
operations: [getBatchPanelPower, getBatchCircuitPower, getCircuitPower]
---

# Break down building power to circuits with Verdigris

Locate load hotspots by walking the electrical hierarchy from panels down to individual circuits. Power is in watts (W).

## 1. Authenticate
Obtain a Bearer token from `https://auth.verdigris.co/oauth/token` (client-credentials, `audience=https://api.verdigris.co/`). Reuse it for 1 hour.

## 2. Panel-level power
`getBatchPanelPower` → `GET /power/panels?panelIds=...` for the panels in the building, over your `start_time`/`end_time` and `interval`.

## 3. Circuit-level power
- Many circuits at once: `getBatchCircuitPower` → `GET /power/circuits?circuitIds=...`.
- One circuit in detail: `getCircuitPower` → `GET /power/circuits/{id}` (real W, apparent VA, reactive VAr).

## 4. Rank and report
Aggregate each circuit's average power across the range and rank descending; use `limit` to cap to the most-recent N points when scanning.

Auth, pagination, and error conventions: `conventions/verdigris-technologies-conventions.yml`.
