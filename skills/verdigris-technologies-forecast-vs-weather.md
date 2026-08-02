---
name: Compare Verdigris energy forecast against weather
description: Retrieve forecasted building energy alongside weather to explain or validate the forecast.
api: openapi/verdigris-technologies-data-v4-openapi.json
operations: [getBatchBuildingForecast, getBatchWeather]
---

# Compare Verdigris energy forecast against weather

Pair Verdigris's energy forecast with the weather series for the same buildings to sanity-check demand predictions.

## 1. Authenticate
Get a client-credentials Bearer token from `https://auth.verdigris.co/oauth/token` (`audience=https://api.verdigris.co/`); cache for 1 hour.

## 2. Forecast
`getBatchBuildingForecast` → `GET /forecast/buildings?buildingIds=...` for forecasted energy across one or more buildings.

## 3. Weather
`getBatchWeather` → `GET /weather/buildings?buildingIds=...` for the matching weather series (temperature in °C).

## 4. Align and analyze
Request both with the same `start_time`/`end_time`/`interval` and `timestampFormat=ISO8601` so points line up, then correlate forecast load against temperature.

Handle 401/404/422 per `errors/verdigris-technologies-problem-types.yml`.
