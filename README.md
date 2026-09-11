# cloud-itonami-isco-9623

Open Occupation Blueprint for **ISCO-08 9623**: Meter Readers and
Vending-machine Collectors.

This repository designs a forkable OSS business for a meter-reading-
and-vending-machine-collection route scheduling and logistics
coordination practice: a route scheduling and supply-coordination
robot manages crew/route records under a governor-gated actor, so a
meter-reading and vending-machine collection crew keeps its own
operating records instead of renting a closed workforce-management
SaaS.

**Maturity: `:implemented`.** `src/meterread/` implements the
`MeterReadingActor` as a `langgraph.graph/state-graph`
(`meterread.actor`) wired to a `Meter Reading and Vending-machine
Collection Advisor` (`meterread.advisor`) and an independent
`MeterReadingGovernor` (`meterread.governor`), following the itonami
actor pattern (ADR-2607121000): `:intake -> :advise -> :govern ->
:decide -+-> :commit (:ok?) +-> :request-approval (:escalate?,
human-in-the-loop interrupt) +-> :hold (:hard?)`. HARD invariants
(always hold, never overridable): worker provenance, route provenance,
no-actuation (`:effect` must be `:propose`), a closed op-allowlist
(`:log-work-record`, `:schedule-crew-operation`,
`:flag-safety-concern`, `:coordinate-supply-order` — nothing else may
ever be proposed). 24 tests / 52 assertions green (`kbb -M:test`).
There is a permanent, unconditional block on any
proposal that would directly finalize a cash-collection/
reconciliation-execution decision (e.g. approving a specific cash
pickup or reconciliation) *or* a site-safety-clearance decision (e.g.
declaring a site cleared for entry), or that would override a route
safety supervisor's judgment. Always-escalate paths (human sign-off
regardless of confidence, mapping this repo's Trust Controls in
[`docs/business-model.md`](docs/business-model.md)):
`:flag-safety-concern` (always) and `:coordinate-supply-order` above
the registered cost threshold.

## Robotics premise

All cloud-itonami verticals are designed on the premise that a **robot
performs the physical domain work**. Here a route scheduling/logistics
coordination robot performs crew scheduling, meter-reading/
collection-log/progress-record logging and route-equipment/
consumables procurement coordination for a meter-reading and
vending-machine collection crew, under an actor that proposes actions
and an independent **MeterReadingGovernor** that gates them. The
governor never dispatches hardware itself, never enters a customer
site, reads a meter or collects cash itself, and never finalizes a
cash-collection/reconciliation-execution decision or a
site-safety-clearance decision, and never overrides a route safety
supervisor's judgment; `:high`/`:safety-critical` actions (such as a
flagged site-access/personal-safety/equipment-condition concern, or an
above-threshold supply order) require human sign-off. **This actor
coordinates ROUTE SCHEDULING/LOGISTICS ONLY — it never enters a
customer site, reads a meter or collects cash itself, and never makes
a site-safety-clearance decision itself.**

## Core Contract

```text
worker roster + route registration + safety-reporting policy
        |
        v
Meter Reading and Vending-machine Collection Advisor -> MeterReadingGovernor -> log/schedule/coordinate, or human sign-off
        |
        v
robot actions (gated) + operating records + audit ledger
```

No automated advice can dispatch a robot action the governor refuses,
finalize a cash-collection/reconciliation-execution decision, finalize
a site-safety-clearance decision (e.g. declaring a site cleared for
entry), override a route safety supervisor's judgment, suppress an
operating record, or disclose sensitive data without governor approval
and audit evidence.

## Capability layer

Resolves via [`kotoba-lang/occupation`](https://github.com/kotoba-lang/occupation)
(ISCO-08 `9623`). Required capabilities:

- :robotics
- :identity
- :audit-ledger

See [`docs/business-model.md`](docs/business-model.md) and
[`docs/operator-guide.md`](docs/operator-guide.md).

## License

AGPL-3.0-or-later.
