# Airport Map State Consistency Plan

## Summary

Implement the airport map so aircraft movement is driven by the same normalized flight status shown on the board. Departures stay parked at their gate until status becomes `Taxiing`; after status becomes `Departed`, the aircraft remains visible on the map for 5 real-time seconds, then disappears from the map only.

## Key Changes

- Add a shared airport status normalization path used by both table rendering and map movement.
- Treat known manual statuses as authoritative: `Scheduled`, `Check-in`, `Gate open`, `Boarding`, `Final call`, `Gate closed`, `Taxiing`, `Departed`, `Expected`, `En route`, `On approach`, `Landed`, `Arrived`, `Cancelled`, `Diverted`.
- For unknown manual status text, fall back to current time-based computed status.
- Update departure map behavior:
  - Pre-taxi states stay fixed at the assigned gate.
  - `Taxiing` moves from gate toward assigned runway.
  - `Departed` shows briefly past/toward runway exit, then hides after 5 real-time seconds.
  - The board row can still show the departed flight according to existing table visibility rules.
- Add transient runtime tracking for first-seen `Departed` timestamps; do not persist this timer to localStorage.
- Rework FRA runway assignment so runway `18` is departure-only:
  - Arrivals must never be routed to `18`.
  - Departures may use `18`.
  - If no valid arrival runway is available, fall back to a non-18 runway before using any generic fallback.
- Redraw the FRA SVG map toward a more geographic static schematic: better relative placement of parallel east-west runways, runway 18, terminals/aprons, gates, and taxi routes, without external map tiles or assets.
- Replace the current plane shape with custom FR24-like SVG silhouettes:
  - Yellow aircraft icon with black/dark outline and rotation.
  - Preserve engine-count distinction.
  - Use at least 2-engine and 4-engine variants based on existing aircraft type/model detection.
  - Do not copy proprietary Flightradar24 asset paths exactly.

## Interfaces / Types

- Keep the app single-file in `index.html`.
- Add small helper functions only inside the existing script, likely around current airport status and movement helpers:
  - `normalizedFlightStatus(flight, referenceMinutes)`
  - `isFlightTaxiing(status)`
  - `isFlightDeparted(status)`
  - runway eligibility helpers for departure/arrival routing.
- Add non-persisted state for departed map hiding, e.g. an object keyed by flight id with wall-clock timestamps.

## Test Plan

- Use Playwright by opening `index.html` directly, no server.
- Verify airport mode loads without console errors.
- FRA departure before `Taxiing`: aircraft remains at gate while board shows pre-taxi states.
- FRA departure at `Taxiing`: aircraft starts moving along taxi route.
- FRA departure at `Departed`: aircraft disappears from map after 5 real-time seconds.
- FRA arrival routing: no arrival aircraft is assigned to runway `18`.
- FRA departure routing: runway `18` remains available for departures.
- Verify 2-engine and 4-engine aircraft render different FR24-like icons.
- Exercise both train and airport modes to ensure train map behavior is unchanged.
- Verify import/export and local state persistence still work after reload; departed 5-second map timers are intentionally not persisted.

## Assumptions

- "5 seconds" means real wall-clock seconds after `Departed` is first rendered.
- "Exact Flightradar24 icons" is implemented as a close custom FR24-like visual, not a copied proprietary asset.
- "Mapa Frankfurt" means a static SVG schematic that is as geographically faithful as practical inside the current single-file app, not embedded real map data.
