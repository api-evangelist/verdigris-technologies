---
name: Pull building energy from Verdigris
description: Authenticate to the Verdigris Data API and retrieve energy consumption for one or many buildings over a time range.
api: openapi/verdigris-technologies-data-v4-openapi.json
operations: [getBuildingEnergy, getBatchBuildingEnergy]
---

# Pull building energy from Verdigris

Use this to fetch building-level energy (Wh for `type=real`, VAh for `type=apparent`) from the Verdigris Data API.

## 1. Get an access token
POST to `https://auth.verdigris.co/oauth/token` (form-encoded) with:
`grant_type=client_credentials`, `client_id`, `client_secret`, `audience=https://api.verdigris.co/`.
Cache the returned JWT for its 1-hour lifetime — the token endpoint allows only 20 requests/hour.

## 2. Query energy
- Single building: `getBuildingEnergy` → `GET /energy/buildings/{id}`.
- Many buildings in one call: `getBatchBuildingEnergy` → `GET /energy/buildings?buildingIds=...`.

Send `Authorization: Bearer <token>`. Provide `start_time` / `end_time` (ISO 8601) and an `interval`
(`1m`, `15m`, `1h`, `1d`, `1M`). Set the `Accept` header to `application/json` (or `text/csv`).

## 3. Handle results and errors
- 401 `UnauthorizedError` → token missing/expired; re-authenticate.
- 404 `EntityNotFoundError` → check `invalidIds[]`; the building id is not in your account.
- 422 `UnprocessableEntityError` → fix the flagged time-range / interval params.

See `conventions/verdigris-technologies-conventions.yml` and `errors/verdigris-technologies-problem-types.yml`.
