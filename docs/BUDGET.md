# Budget & Expense Management

(Same process as the fall-detector project — adapted for this
project's higher hardware cost.)

## Roles

- **Treasurer:** logs every purchase, tracks running totals against
  budget, flags overspend early. Separate from Project Lead to reduce
  conflict-of-interest friction.

## Funding

- This project has meaningfully higher hardware costs than a wearable
  (autopilot board, motors/ESCs, sonar module, multiple water-quality
  probes, hull materials, battery). Strongly consider pursuing a
  small department/faculty/sponsor grant before assuming this is all
  out-of-pocket — the real-data/real-impact framing (mapping
  previously unsurveyed local coastline) is a reasonable grant pitch.
- Rough per-sub-team allocation (revisit monthly):
  - Hull/Mechanical: hull materials are cheap; biggest cost here is
    waterproof enclosures/connectors, not the hull itself
  - Propulsion & Power Electronics: significant share — motors, ESCs,
    battery, BMS
  - Embedded/Autopilot: significant share — autopilot board, GPS
    module, telemetry radio
  - Sensing: significant share — sonar module (can be pricier),
    water-quality probes (DO and pH probes are the priciest — treat
    as first cut if budget is tight)
  - Backend/Dashboard: mostly free (software), maybe minor cloud
    hosting if beyond free tiers

## Approval process

- Purchases **below** an agreed threshold (suggest EUR 30-50): the
  relevant sub-team lead approves within their own team's allocated
  budget.
- Purchases **above** the threshold, or any purchase that's a shared/
  cross-team item (e.g. the sonar module, the autopilot board): get a
  quick async approval from the Project Lead/Treasurer before buying.
- Log every purchase in `docs/expenses.csv` before or immediately
  after buying. Keep receipts — useful for reimbursement and any
  grant/sponsor accounting.

## Reimbursement

- Whoever buys something on their own money logs it in
  `docs/expenses.csv` with status `pending_reimbursement`.
- Treasurer settles up on an agreed cadence (e.g. monthly) from the
  shared fund, updates status to `reimbursed`.

## Monthly review

At each monthly milestone check-in, the Treasurer reports: total
spent so far, spent per sub-team, remaining budget, and known
upcoming costs (e.g. the sonar module purchase, DO probe if budget
allows). Adjust allocation if one sub-team is running hot — this
project's costs are more front-loaded (most big-ticket items are
bought in Month 1) than the fall-detector's, so watch the early
months closely.
