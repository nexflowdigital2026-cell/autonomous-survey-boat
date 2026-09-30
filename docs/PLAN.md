# Autonomous Bathymetric & Water-Quality Survey Boat (ASV) — Project Plan

## Problem

Shallow coastal water, small harbors, lakes, and river mouths are
frequently unsurveyed or only surveyed rarely and expensively (manned
boats + professional sonar crews). Greece has a huge amount of
coastline and thousands of small harbors/coves that have no modern
depth map or water-quality baseline. This data matters for navigation
safety, environmental monitoring (pollution, algae blooms, temperature
trends), fisheries, and coastal engineering — but nobody surveys it
because it's not economical to send a crewed survey boat to a small
bay.

## Goal

Build an autonomous surface vehicle (ASV) — a small self-driving boat —
that autonomously drives a survey pattern over a body of water while
logging: bottom depth (bathymetry), water quality (temperature, pH,
turbidity, dissolved oxygen), and GPS-tagged readings for every sample,
producing a real, publishable map of a previously unsurveyed area.

Scope for this 6-month phase: **surface vehicle only** (no full
submersion). A tethered/winched sensor pod that can be lowered a few
meters below the hull is an optional stretch goal for closer
inspection at a specific waypoint. A fully autonomous submersible
(true AUV) is explicitly future work — not attempted this phase (see
"Explicitly out of scope" below).

## Why a surface vehicle instead of a submarine (design rationale)

Underwater, you lose GPS (no radio propagation through water), lose
WiFi/radio communication beyond a few cm, and take on real pressure-
hull engineering and buoyancy control. A surface vehicle keeps GPS the
entire mission, uses ordinary radio telemetry the entire mission, and
needs no pressure hull at all — while a downward-facing sonar still
maps the sea floor beneath it, exactly like real hydrographic survey
boats do. This removes the two hardest problems in underwater robotics
(localization without GPS, and communication without radio) while
still delivering the actual goal: real seafloor and water-quality data.

## System overview

```
                     [ Shore / Base Station ]
                      laptop running dashboard
                              ^  |
                   telemetry radio (RC-style, long range)
                              |  v
   +------------------------------------------------------+
   |                  ASV (catamaran hull)                |
   |                                                        |
   |  Flight/drive controller: ArduPilot "Rover" firmware  |
   |  on a Pixhawk-class autopilot board                   |
   |                                                        |
   |  Sensors:                                             |
   |   - GPS module           (position)                   |
   |   - Compass/IMU          (heading, already on autopilot)|
   |   - Downward sonar       (bottom depth / bathymetry)  |
   |   - Water quality probes (temp, pH, turbidity, DO)    |
   |   - (stretch) winch + underwater camera pod           |
   |                                                        |
   |  Propulsion: twin brushless motors + marine props,    |
   |  one per pontoon, differential steering               |
   |                                                        |
   |  Power: LiPo battery bank, waterproof enclosure        |
   +------------------------------------------------------+
```

## Team structure (13 people, 5 sub-teams)

1. **Hull/mechanical (3):** catamaran hull design and build (3D-printed
   or plywood/foam composite), pontoon layout, sensor/probe mounting
   (boom below the hull), waterproof electronics enclosure, motor
   mounts, (stretch) winch mechanism for the lowered sensor pod.
2. **Propulsion & power electronics (2):** brushless motor + ESC
   selection and wiring, battery sizing and management (LiPo + BMS),
   waterproof cable glands/connectors, power budget across all
   subsystems.
3. **Embedded / autopilot integration (3):** ArduPilot Rover setup and
   tuning, GPS + compass + IMU integration, telemetry radio link,
   waypoint mission configuration, safety features (return-to-home,
   low-battery abort, geofence).
4. **Sensing (2):** downward sonar/depth sensor integration and
   logging, water quality probe integration and calibration, sensor
   fusion/data timestamping and GPS-tagging of every reading.
5. **Backend / dashboard / mapping (3):** live telemetry dashboard
   (position, depth, sensor readings, battery, mission status),
   post-mission map generation (turn logged depth+GPS+water-quality
   points into an actual 2D/3D map/heatmap), data export/storage.

One person from the group acts as overall Project Lead (cross-cutting,
same governance model as the fall-detector project — see that repo's
`docs/GOVERNANCE.md` for the reusable leadership/decision-making/
budget process, which applies here unchanged).

## Sensors — "every kind that provides useful information"

Core (build the MVP around these):
- **GPS module** — position for every logged sample (u-blox NEO-M8N
  class, cheap and well-supported by ArduPilot).
- **Compass + IMU** — heading and orientation, typically bundled with
  the GPS module or the autopilot board itself.
- **Downward-facing sonar / depth sounder** — bathymetry (the core
  "fish finder" capability). Single-beam to start (simpler, cheaper),
  multi-beam or side-scan as a stretch goal for richer seafloor
  imaging.
- **Water temperature probe** — cheap, waterproof, high value (thermal
  stratification, pollution/discharge detection).
- **Turbidity sensor** — water clarity, indicates sediment/pollution.
- **pH sensor** — water chemistry / pollution indicator.
- **Dissolved oxygen (DO) sensor** — critical for ecosystem health
  monitoring (fish kills, algae bloom risk) — pricier than the others,
  treat as a "nice to have if budget allows" rather than MVP-critical.

Stretch (add once the MVP survey mission works end-to-end):
- **Conductivity/salinity probe** — useful near river mouths / for
  detecting freshwater-saltwater mixing zones.
- **Downward-facing camera** — visual seafloor imagery in clear
  shallow water, correlates with sonar readings.
- **Underwater camera on a lowered winch pod** — the "genuine
  underwater element" without full submersion risk; useful for visual
  inspection at a specific point of interest flagged by sonar/water
  quality anomalies.
- **Weather station on the base station** (wind speed/direction, air
  temp) — context data, and wind speed matters a lot for a small
  boat's safe operating envelope.

## Month-by-month plan

**Month 1 — Foundations**
- Finalize hull design (catamaran, dimensions sized to your budget/
  transportability — something that fits in a car trunk is a practical
  constraint worth deciding now)
- Order core parts: autopilot board, GPS, motors/ESCs, battery, hull
  material
- Flash ArduPilot Rover on the autopilot board, bench-test manual RC
  control of the motors (no water yet)
- Define interface contracts between sub-teams (telemetry data format,
  sensor logging schema, dashboard API) — see fall-detector repo's
  `docs/decisions/TEMPLATE.md` for the format to reuse

**Month 2 — First water test (manual control)**
- Hull assembled, motors/props mounted, waterproof enclosure sealed
- First in-water test: manual RC control only, confirm it drives
  straight, turns correctly, doesn't take on water, motors don't
  overheat/stall
- GPS + telemetry radio link working, live position shown on a basic
  dashboard
- Sonar module bench-tested in a bucket/pool for basic depth reading
  sanity check

**Month 3 — Sensor integration**
- Sonar mounted and logging real depth data during manual drives
- Water quality probes mounted and logging (temperature, turbidity,
  pH at minimum)
- Every logged sample timestamped and GPS-tagged
- Dashboard shows live sensor readings alongside position

**Month 4 — Autonomy**
- ArduPilot Rover waypoint missions configured and tested (drive a
  simple back-and-forth "mow the lawn" survey grid autonomously)
- Safety behaviors verified: return-to-home on lost signal, low-battery
  abort, geofence boundary
- Full end-to-end autonomous mission: boat drives a survey grid
  unattended, logs all sensor data, returns safely

**Month 5 — Real field survey**
- Select a real, previously-unsurveyed shallow-water site (a small
  harbor, cove, or lake near you)
- Run full autonomous survey missions, collect real depth + water-
  quality data across the site
- Post-process logged data into an actual bathymetric map and
  water-quality overlay (this is your real deliverable/dataset)
- Iterate based on real-world issues found (waterproofing leaks, sonar
  noise, GPS accuracy, battery life)

**Month 6 — Polish & presentation**
- Finalize the dashboard/map visualization
- Write up methodology and findings (what did you actually map, what
  does the data show)
- Publish the dataset/map (a genuine open dataset of a real place is a
  strong deliverable for a sponsor/department presentation)
- Demo video, poster, final presentation

## Explicitly out of scope for this phase (be upfront about this)

- Full submersion / true AUV operation
- Underwater acoustic communication
- Operating in open ocean / rough sea conditions or at any real depth
  or distance from shore beyond calm, shallow, sheltered water
- Multi-vehicle/swarm operation (this is a single-vehicle project)

These are reasonable "phase 2" pitches if the team wants to continue
past 6 months, but promising them now would overcommit the project.

## Budget shape (rough, refine once you price real parts)

- Hull/mechanical: largest single-item cost is usually the autopilot
  board + GPS module + motors/ESCs, not the hull material itself
  (plywood/foam/3D-printed hull is cheap)
- Sonar module: a meaningful cost item — start with the cheapest
  workable single-beam unit, upgrade later if the budget allows
  multi-beam
- Water quality probes: DO and pH probes are the pricier sensors, treat
  as the first thing to cut if budget is tight, add back once other
  parts are validated

## Reused infrastructure from the fall-detector project

- Same governance model (Project Lead + sub-team Technical Leads,
  interface-contract-first collaboration, weekly sync + bi-weekly
  demo cadence) — see the fall-detector repo's `docs/GOVERNANCE.md`.
- Same budget/expense tracking process — see `docs/BUDGET.md` and the
  `docs/expenses.csv` pattern.
- Same GitHub workflow (branch per sub-team/feature, PR review, Project
  board as source of truth) — see `CONTRIBUTING.md`.
