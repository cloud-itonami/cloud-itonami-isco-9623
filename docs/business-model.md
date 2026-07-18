# Business Model: Meter Reading and Vending-machine Collection Route Scheduling Coordination Practice

## Classification

- Repository: `cloud-itonami-isco-9623`
- ISCO-08: `9623`
- Occupation: Meter Readers and Vending-machine Collectors
- Social impact: worker-safety, billing-accuracy, cash-handling-integrity

## Customer

- utility (electricity/water/gas) meter-reading operators
- vending-machine operators and route-service companies

## Offer

- crew shift/route task scheduling coordination
- meter-reading/collection-log/progress-record logging
- route-equipment/consumables procurement coordination
- safety-concern surfacing to route safety supervisors

## Revenue

- monthly retainer
- per-crew coordination fee

## Trust Controls

- no direct finalization of a cash-collection/reconciliation-execution
  decision (e.g. approving a specific cash pickup or reconciliation),
  ever
- no direct finalization of a site-safety-clearance decision (e.g.
  declaring a site cleared for entry), ever
- no override of a route safety supervisor's judgment, ever
- flagged safety concerns (site-access/personal-safety hazard,
  equipment-condition hazard) always route to human sign-off,
  regardless of confidence
- no supply order above the registered cost threshold without
  governor-gated human sign-off
- operating and coordination records are auditable, not editable
- a logged work record is administrative route/reading/collection-log/
  progress metadata only — never a cash-amount reconciliation
  determination
