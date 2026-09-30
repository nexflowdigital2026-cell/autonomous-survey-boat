# Team Governance & Workflow

How this project is organized. Read this before your first
contribution. (Adapted from the fall-detector project's governance
model — same principles, applied to a 5-sub-team, 13-person project.)

## Leadership

- **Project Lead (1):** owns the roadmap/milestones, runs the weekly
  sync, breaks cross-team ties, owns budget approval, manages external
  relationships (faculty advisor, sponsors, suppliers, test site
  access for field surveys).
- **Technical Lead per sub-team (1 each):** owns technical decisions
  within their sub-team's domain, commits to and maintains that team's
  interface contract (see below), reports status/blockers at the
  weekly sync. Rotates roughly every 2 months if people want the
  leadership line on their CV.

Sub-teams (see docs/PLAN.md for full scope of each):
1. Hull / Mechanical
2. Propulsion & Power Electronics
3. Embedded / Autopilot Integration
4. Sensing
5. Backend / Dashboard / Mapping

## Decision-making

- Decisions inside a sub-team's own domain: made by that sub-team's
  lead, no escalation needed.
- Cross-team decisions (e.g. the telemetry data format both the
  autopilot team and the dashboard team depend on): proposed by the
  upstream owner, reviewed by the downstream owner, written down as a
  short doc/PR in this repo. If the two leads can't agree within 48
  hours, escalate to the Project Lead.
- Every non-trivial cross-team decision gets a short write-up (a
  markdown file under `docs/decisions/` or a PR description) — don't
  let it live only in a chat thread.

## Interface contracts (this is what lets teams work independently)

Each sub-team publishes a short spec for what it produces, BEFORE the
real implementation is finished, so other teams can build against the
spec/a mock instead of waiting:

- **Hull/Mechanical -> Propulsion:** mounting points, weight budget,
  motor/prop clearance.
- **Propulsion -> Embedded:** power available, motor control interface
  (PWM ranges, ESC protocol).
- **Embedded/Autopilot -> Backend/Dashboard:** telemetry data format
  sent over the radio link (position, heading, battery, mission
  status, sensor readings) — publish this schema before the real
  telemetry stream exists, so the dashboard team can build against a
  mock feed immediately.
- **Sensing -> Embedded/Autopilot:** how sonar/water-quality readings
  are exposed to the autopilot's data logger (format, sample rate,
  units).
- **Sensing -> Backend/Dashboard:** the GPS-tagged sensor reading
  schema used for post-mission map generation.

Template for a contract doc: `docs/decisions/TEMPLATE.md` (same
template as the fall-detector project).

## Git workflow

See `CONTRIBUTING.md`.

## Communication cadence

- **Daily async standup:** one message per person in the team chat —
  yesterday / today / blockers.
- **Weekly sync (30-45 min, whole team):** each sub-team lead gives a
  ~3 minute status; blockers and cross-team decisions surfaced here.
- **Bi-weekly demo:** show whatever is working, live, to the whole
  team — even if it's just "motors respond to RC input" in month 1.
  Keeps momentum over 6 months.

## Budget & expenses

See `docs/BUDGET.md` for the process and `docs/expenses.csv` for the
running ledger. This project has meaningfully higher hardware costs
than the fall-detector project (multiple motors, an autopilot board,
a sonar module, several water-quality probes, a hull) — pursuing a
department/sponsor grant is worth prioritizing early (see BUDGET.md).

## Safety note

This project involves water operation and a boat with spinning
propellers. Basic safety practices: never test in open/rough water
alone, always have a second person present during in-water testing,
keep a kill-switch/manual-override accessible during all autonomous
mission tests, and check local regulations for operating any
autonomous vehicle on public waterways before field testing.
