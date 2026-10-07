# Zava DIY — make-to-stock pilot: decision log

Twelve decisions from three discovery workshops, scoping the single-item
make-to-stock pilot this lab configures in Dynamics 365 (an assembled tool
cabinet, built from three raw materials on a two-operation route). This is
the fictional equivalent of a client decision log: the kind of source
document a runbook cites so a reviewer can trace every configuration action
back to *why* it's being done, not just *what* gets clicked.

| ID | Topic | Decision | Rationale | Workshop session | Status |
|---|---|---|---|---|---|
| D-01 | Scope | Pilot covers one finished item, one production route, single site. No multi-site or multi-currency until the pilot proves out. | Keep the first AI-assisted configuration cycle small enough to validate end to end in one sprint. | Session 1 | Approved |
| D-02 | Sourcing | Acme Metal Supply (VEND-001) is sole source for all three raw materials during the pilot. | Avoids splitting receipts across vendors while the team is still learning batch tracking; multi-vendor sourcing is explicitly out of scope for this phase. | Session 1 | Approved |
| D-03 | Costing method | Raw materials are costed at standard cost, not actual/FIFO. | Standard costing isolates production variance (labor, scrap) from purchase price variance — the team wants the pilot's cost-variance reporting to be unambiguous about which side of the flow a variance came from. | Session 1 | Approved |
| D-04 | Batch tracking | Every raw material receipt and every finished-good receipt is batch tracked, with full genealogy from PO through consumption to sale. | Required for the recall traceability the retail customer contract mandates, and it's the only way to run the reconciliation checks below. | Session 1 | Approved |
| D-05 | BOM | Approved BOM (`BOM-FG100`) is 4 Steel Panel : 2 Hinge Kit : 1 Lock Assembly per cabinet. | Matches the engineering drawing signed off in the prior design review; not re-litigated in these workshops. | Session 2 | Approved |
| D-06 | Routing | Two operations only: Cutting then Assembly. No subcontracted operations in the pilot. | Both operations run on owned equipment at the pilot site; subcontracting a step would introduce a vendor cost variance the team wants to keep out of scope for now. | Session 2 | Approved |
| D-07 | Route cost rates | Cutting at $45.00/hr, Assembly at $60.00/hr. | Assembly requires a certified operator and carries the higher labor rate; the team flagged Assembly as the operation most likely to show hour overruns on a new item, and wanted its rate set correctly from day one so any future variance is a hours story, not a rate story. | Session 2 | Approved |
| D-08 | Variance materiality | A cost element (Material, or a route operation) is investigated before month-end close if its variance exceeds 10% of its estimate. | Below 10%, the team judged the noise-to-signal ratio too low to be worth a manual look each month; above it, Finance wants a named driver before the close checklist is signed off. | Session 2 | Approved |
| D-09 | Customer scope | Riverside Builders (CUST-001) is the only customer for this item during the pilot. | The item is being piloted with one existing B2B account before a wider retail rollout. | Session 3 | Approved |
| D-10 | Pricing floor | **Open item.** Sales currently prices against `Item.StandardCost` ($83.90), not realized production cost. The team acknowledged this can let a line go to the floor — or below actual cost — without anyone noticing until month-end. | Raised in Session 3 when Finance pointed out that a production variance never flows back into the price the sales desk quotes. No fix was agreed in-session; flagged as a gap for the runbook to surface, not resolve. | Session 3 | **Open — not yet actioned** |
| D-11 | Pricing floor automation | Once realized-cost margin reporting is running reliably (i.e., after this pilot), revisit D-10 and consider a hard floor check before order confirmation. | Sequencing decision — the team didn't want to design a pricing control before they trust the underlying cost numbers it would be built on. | Session 3 | Deferred, pending D-10 |
| D-12 | Reconciliation cadence | Run the batch-receipt-vs-consumption integrity check (no batch consumed before its own receipt date) before every close, not just at go-live. | Session 1 already surfaced one instance of a consumption transaction backdated ahead of its batch's receipt — traced to a data-entry timing issue, not a process failure — and the team wants it caught automatically going forward rather than found by accident again. | Session 1 | Approved |

**Note on D-10 and D-12:** these two are deliberately left open, not
resolved by this pilot's configuration. They're included so the generated
runbook has to decide what to do with a real gap — flag it and move on,
rather than silently configure past it — the same way a live engagement
would.
