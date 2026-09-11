(ns meterread.store
  "SSoT for the ISCO-08 9623 meter-reading-and-vending-machine-
  collection-route scheduling/logistics coordination actor (itonami
  actor pattern, ADR-2607121000 / CLAUDE.md Actors section; README's
  'Robotics premise' — a route scheduling/logistics coordination robot
  manages crew scheduling, meter-reading/collection-log/progress-record
  logging and route-equipment/consumables procurement coordination for
  a meter-reading and vending-machine collection crew under this
  advisor/governor pair, which never dispatches hardware itself, never
  enters a customer site or reads a meter or collects cash itself, and
  never finalizes a cash-collection/reconciliation-execution decision
  or a site-safety-clearance decision, and never overrides a route
  safety supervisor's judgment — those remain the route safety
  supervisor's exclusive judgment). Modeled closely on
  cloud-itonami-isco-9611's wastecollect.store for the outdoor
  route-based elementary-occupation hazard-domain shape, extended with
  a second, independent cash-handling trust/security hazard-scope
  dimension (meter readers and vending-machine collectors access
  customer sites — a site-access/personal-safety hazard from entering
  unfamiliar premises — and vending-machine collectors also handle cash
  from vending machines — a cash-handling trust/security hazard — so
  both dimensions stack independently on top of each other, mirroring
  9611's dual vehicle-traffic-hazard + hazardous-material-handling-
  hazard stack).

  Domain:

    worker — a registered meter-reading/vending-machine-collection crew
             member (:worker-id, :name)
    route  — a registered meter-reading/vending-machine-collection
             route {:route-id :name :max-supply-cost number}.
             `:max-supply-cost` is an informational registered ceiling
             used only to decide whether a `:coordinate-supply-order`
             proposal escalates to human sign-off (the governor never
             blocks a within-threshold order outright; it only decides
             commit vs. escalate).
    record — a committed operating record (a logged meter-reading/
             collection-log/progress entry, a scheduled crew operation,
             a flagged safety concern, or a coordinated supply order)
             — written ONLY via commit-record!. A logged work record is
             administrative route/reading/collection-log/progress
             metadata ONLY — it is never a cash-amount reconciliation
             determination (that decision is permanently out of this
             actor's scope, see meterread.governor).
    ledger — append-only audit trail, commit or hold.")

(defprotocol Store
  (worker [s worker-id])
  (route [s route-id])
  (records-of [s worker-id])
  (ledger [s])
  (register-worker! [s worker])
  (register-route! [s route])
  (commit-record! [s record])
  (append-ledger! [s fact]))

(defrecord MemStore [a]
  Store
  (worker [_ worker-id] (get-in @a [:workers worker-id]))
  (route [_ route-id] (get-in @a [:routes route-id]))
  (records-of [_ worker-id] (filter #(= worker-id (:worker-id %)) (:records @a)))
  (ledger [_] (:ledger @a))
  (register-worker! [s w]
    (swap! a assoc-in [:workers (:worker-id w)] w) s)
  (register-route! [s r]
    (swap! a assoc-in [:routes (:route-id r)] r) s)
  (commit-record! [s record]
    (swap! a update :records (fnil conj []) record) s)
  (append-ledger! [s fact]
    (swap! a update :ledger (fnil conj []) fact) s))

(defn mem-store
  ([] (mem-store {}))
  ([seed] (->MemStore (atom (merge {:workers {} :routes {} :records [] :ledger []}
                                    seed)))))
