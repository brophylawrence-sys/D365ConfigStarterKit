# Runbook: Zava DIY make-to-stock pilot — end-to-end item configuration

**Purpose:** Configure one make-to-stock finished item (the assembled tool
cabinet, `FG-CABINET-100`) end to end in Dynamics 365 Supply Chain
Management — released products, BOM, route, and a production order ready to
estimate, start, and report as finished — so the pilot scoped in the three
discovery workshops can be validated in the system.
**Trigger:** Phase 2 execution of this pilot, once this runbook is reviewed
and approved by the implementation team.
**Source:**
- *How* — MS Learn MCP (this run: MS Learn MCP access was not authorized in
  this session, so the cited pages were located via web search scoped to
  `learn.microsoft.com`; re-verify each citation against MS Learn MCP before
  Phase 2 execution).
- *What* (values) — `reference/zava_item_bom_master.xlsx` (Items, BOM,
  TradingPartners sheets) and `reference/zava_route_rates.xlsx`
  (RouteOperations, WorkCenterRates sheets), computed/formula cells only.
- *Why* — `discovery/decision-log.md` (decision IDs D-01–D-12) and
  `discovery/workshop-transcripts.md` (Sessions 1–3).
**Skill(s) used:** make-to-stock-configuration (`.github/skills/make-to-stock-configuration/skill.md`)

## Configuration data at a glance

| Entity | ID | Key values | Source |
|---|---|---|---|
| Raw material | RM-STEEL-001 | Steel Panel 600x400, ea, item model group `RawMat-Batch`, Standard cost, StandardCost $12.50 | Items sheet; D-03, D-04 |
| Raw material | RM-HINGE-002 | Heavy Duty Hinge Kit, ea, `RawMat-Batch`, Standard cost, StandardCost $3.20 | Items sheet; D-03, D-04 |
| Raw material | RM-LOCK-003 | Cabinet Lock Assembly, ea, `RawMat-Batch`, Standard cost, StandardCost $8.75 | Items sheet; D-03, D-04 |
| Finished good | FG-CABINET-100 | Assembled Tool Cabinet, ea, item model group `FG-Batch`, Standard cost, MaterialCostPerUnit $65.15, RouteCostPerUnit $18.75, **StandardCost $83.90** | Items sheet; D-01, D-05, D-06, D-07 |
| BOM | BOM-FG100 | 4 × RM-STEEL-001 ($50.00) + 2 × RM-HINGE-002 ($6.40) + 1 × RM-LOCK-003 ($8.75) = $65.15 | BOM sheet; D-05 |
| Route | ROUTE-FG100 | Op 10 Cutting 0.15 hr/unit @ $45.00/hr = $6.75; Op 20 Assembly 0.20 hr/unit @ $60.00/hr = $12.00; **total $18.75/unit** | RouteOperations sheet; D-06, D-07 |
| Work center rate | Cutting | Machine, $45.00/hr USD | WorkCenterRates sheet; D-07 |
| Work center rate | Assembly | Labor – Certified operator, $60.00/hr USD | WorkCenterRates sheet; D-07 |
| Vendor | VEND-001 | Acme Metal Supply — sole source for all 3 raw materials | TradingPartners sheet; D-02 |
| Customer | CUST-001 | Riverside Builders — sole customer for this item | TradingPartners sheet; D-09 |

FG-CABINET-100's StandardCost ($83.90) = MaterialCostPerUnit ($65.15, from the
BOM roll-up) + RouteCostPerUnit ($18.75, from the route roll-up) — both are
workbook-computed totals, not re-derived here.

## Steps

### 1. Released products (must exist before BOM, route, or production order)

- [ ] Create item model group `RawMat-Batch` with inventory model **Standard
      cost**, and confirm/create a batch-tracked storage or tracking
      dimension group for raw materials (full PO-to-consumption genealogy).
      — MS Learn: [Prerequisites for standard costs](https://learn.microsoft.com/en-us/dynamics365/supply-chain/cost-management/prerequisites-standard-costs); data: `zava_item_bom_master.xlsx!Items` (ItemModelGroup = `RawMat-Batch`); decision: D-03 (standard costing), D-04 (full batch genealogy), Session 1
- [ ] Create item model group `FG-Batch` with inventory model **Standard
      cost**, and a batch-tracked tracking dimension group for the finished
      good.
      — MS Learn: [Prerequisites for standard costs](https://learn.microsoft.com/en-us/dynamics365/supply-chain/cost-management/prerequisites-standard-costs); data: `zava_item_bom_master.xlsx!Items` (ItemModelGroup = `FG-Batch`); decision: D-03, D-04, Session 1
- [ ] Create a costing version of type **Standard cost** (activate for the
      relevant date) that will hold the standard-cost records set below.
      — MS Learn: [Costing versions overview](https://learn.microsoft.com/en-us/dynamics365/supply-chain/cost-management/costing-versions); data: `zava_item_bom_master.xlsx!Items` (CostingMethod = "Standard cost" for all four items); decision: D-03, Session 1
- [ ] Create released product **RM-STEEL-001** — "Steel Panel 600x400", unit
      = ea, item model group `RawMat-Batch`, batch tracking enabled.
      — MS Learn: [Create a released product for a single company](https://learn.microsoft.com/en-us/dynamics365/supply-chain/pim/tasks/create-released-product-single-company); data: `zava_item_bom_master.xlsx!Items` row 1; decision: D-04, D-05, Session 1–2
- [ ] Create released product **RM-HINGE-002** — "Heavy Duty Hinge Kit",
      unit = ea, item model group `RawMat-Batch`, batch tracking enabled.
      — MS Learn: [Create a released product for a single company](https://learn.microsoft.com/en-us/dynamics365/supply-chain/pim/tasks/create-released-product-single-company); data: `zava_item_bom_master.xlsx!Items` row 2; decision: D-04, D-05, Session 1–2
- [ ] Create released product **RM-LOCK-003** — "Cabinet Lock Assembly",
      unit = ea, item model group `RawMat-Batch`, batch tracking enabled.
      — MS Learn: [Create a released product for a single company](https://learn.microsoft.com/en-us/dynamics365/supply-chain/pim/tasks/create-released-product-single-company); data: `zava_item_bom_master.xlsx!Items` row 3; decision: D-04, D-05, Session 1–2
- [ ] Create released product **FG-CABINET-100** — "Assembled Tool
      Cabinet", unit = ea, item model group `FG-Batch`, batch tracking
      enabled.
      — MS Learn: [Create a released product for a single company](https://learn.microsoft.com/en-us/dynamics365/supply-chain/pim/tasks/create-released-product-single-company); data: `zava_item_bom_master.xlsx!Items` row 4; decision: D-01, D-05, D-06, D-07, Session 1–2
- [ ] Complete basic setup on all four released product masters (default
      order settings, site/warehouse assignment for the pilot's single
      site) before referencing them in a BOM, PO, or sales order.
      — MS Learn: [Complete basic setup of a released product master](https://learn.microsoft.com/en-us/dynamics365/supply-chain/pim/tasks/complete-basic-setup-released-product-master); data: `zava_item_bom_master.xlsx!Items`; decision: D-01 (single site), Session 1
- [ ] Enter the standard-cost records for all four items in the active
      standard-cost costing version: RM-STEEL-001 = $12.50,
      RM-HINGE-002 = $3.20, RM-LOCK-003 = $8.75, FG-CABINET-100 = $83.90 —
      then activate the costing version so these become the live standard
      costs.
      — MS Learn: [Prepare to maintain standard costs for manufactured items](https://learn.microsoft.com/en-us/dynamics365/supply-chain/cost-management/prepare-maintain-standard-costs-manufactured-items); [Update standard costs for a new manufactured item](https://learn.microsoft.com/en-us/dynamics365/supply-chain/cost-management/update-standard-costs-new-manufactured-item); data: `zava_item_bom_master.xlsx!Items` column `StandardCost`; decision: D-03, Session 1

### 2. Vendor / customer master (needed before PO/sales order; does not block BOM/route)

- [ ] Create vendor account **VEND-001** — "Acme Metal Supply" — as sole
      source for all three raw material items during the pilot.
      — MS Learn: [Create a vendor account](https://learn.microsoft.com/en-us/dynamics365/supply-chain/procurement/tasks/create-vendor-account); data: `zava_item_bom_master.xlsx!TradingPartners` row 1; decision: D-02, Session 1
- [ ] Create customer account **CUST-001** — "Riverside Builders" — as the
      sole customer for this item during the pilot.
      — MS Learn: [Accounts receivable home page](https://learn.microsoft.com/en-us/dynamics365/finance/accounts-receivable/accounts-receivable) (customer setup entry point; no single MS Learn "create customer" task page was found — confirm the exact task page via MS Learn MCP in Phase 2); data: `zava_item_bom_master.xlsx!TradingPartners` row 2; decision: D-09, Session 3

### 3. Bill of materials (components must be released products first)

- [ ] Create BOM **BOM-FG100** for item FG-CABINET-100 via the BOM
      designer, with lines: 4 ea RM-STEEL-001 (line cost $50.00), 2 ea
      RM-HINGE-002 (line cost $6.40), 1 ea RM-LOCK-003 (line cost $8.75) —
      total component cost $65.15, matching `Items!MaterialCostPerUnit`.
      — MS Learn: [BOM designer functionality](https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/bom-designer-functionality); [Create bill of materials in Dynamics 365 Supply Chain Management (training)](https://learn.microsoft.com/en-us/training/modules/create-bill-materials-dyn365-supply-chain-mgmt); data: `zava_item_bom_master.xlsx!BOM` rows 1–3; decision: D-05, Session 2
- [ ] Approve the BOM version for BOM-FG100 (Approval > Approved by, on the
      BOM version tab).
      — MS Learn: [BOM designer functionality](https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/bom-designer-functionality); data: `zava_item_bom_master.xlsx!BOM`; decision: D-05, Session 2
- [ ] Activate the approved BOM version for BOM-FG100 — a production order
      cannot consume an inactive BOM version.
      — MS Learn: [BOM designer functionality](https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/bom-designer-functionality); data: `zava_item_bom_master.xlsx!BOM`; decision: D-05, Session 2

### 4. Route (operations and work centers must exist and be active before assignment to a production order)

- [ ] Create/confirm work center **Cutting** with cost category "Machine"
      and hourly rate $45.00 USD.
      — MS Learn: [Routes and operations](https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/routes-operations); data: `zava_route_rates.xlsx!WorkCenterRates` row 1; decision: D-06, D-07, Session 2
- [ ] Create/confirm work center **Assembly** with cost category "Labor –
      Certified operator" and hourly rate $60.00 USD.
      — MS Learn: [Routes and operations](https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/routes-operations); data: `zava_route_rates.xlsx!WorkCenterRates` row 2; decision: D-06, D-07, Session 2
- [ ] Create route **ROUTE-FG100** for item FG-CABINET-100, with operation
      10 (`OPR-10`, "Cut steel panels to size", work center Cutting,
      0.15 hr/unit) and operation 20 (`OPR-20`, "Assemble cabinet; fit
      hinges and lock", work center Assembly, 0.20 hr/unit) — no
      subcontracted operations.
      — MS Learn: [Routes and operations](https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/routes-operations); data: `zava_route_rates.xlsx!RouteOperations` rows 1–2; decision: D-06, Session 2
- [ ] Approve and activate the route version for ROUTE-FG100 — resulting
      cost per unit $6.75 (Cutting) + $12.00 (Assembly) = $18.75, matching
      `Items!RouteCostPerUnit`.
      — MS Learn: [Routes and operations](https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/routes-operations); data: `zava_route_rates.xlsx!RouteOperations` total row; decision: D-06, D-07, Session 2

### 5. Production order (requires an active BOM version and an active route; created last)

- [ ] Create a production order for item FG-CABINET-100 against the
      activated BOM-FG100 and ROUTE-FG100.
      **Quantity is not specified in any source file** — the decision log
      and workbooks size the item/BOM/route but not a pilot batch size.
      Recommend a minimal quantity (1 ea) consistent with keeping the first
      end-to-end cycle small (D-01); confirm the actual pilot quantity with
      Operations before Phase 2 execution rather than assuming this
      default.
      — MS Learn: [Production order lifecycle overview](https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/create-production-orders); [Create a production order](https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/tasks/create-production-order); data: `zava_item_bom_master.xlsx!Items` (FG-CABINET-100), `zava_item_bom_master.xlsx!BOM`, `zava_route_rates.xlsx!RouteOperations`; decision: D-01, Session 1 (quantity is a runbook recommendation, not a sourced value — flag before executing)
- [ ] Estimate the production order and confirm the calculated cost per
      unit reconciles to $83.90 ($65.15 material + $18.75 route) before
      release.
      — MS Learn: [Estimate a production order](https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/tasks/estimate-production-order); [Production order cost estimation](https://learn.microsoft.com/en-us/dynamics365/supply-chain/cost-management/production-order-cost-estimation); data: `zava_item_bom_master.xlsx!Items` (StandardCost = 83.90); decision: D-03 (isolates material vs. route/labor variance), Session 1–2
- [ ] Release and start the production order (route card journal generated
      for operation 10, Cutting, first).
      — MS Learn: [Release production orders](https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/release-production-orders); [Production process overview](https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/production-process-overview); data: `zava_route_rates.xlsx!RouteOperations` (operation sequence 10 → 20); decision: D-06, Session 2
- [ ] Report as finished, consuming the batch-tracked component quantities
      per BOM-FG100 and receiving the finished-good batch into inventory.
      — MS Learn: [Report production orders as finished](https://learn.microsoft.com/en-us/dynamics365/supply-chain/production-control/report-production-orders-as-finished); data: `zava_item_bom_master.xlsx!BOM`; decision: D-04 (full batch genealogy from receipt through consumption), Session 1

## Expected result / checkpoint

All four released products exist with standard costs active; BOM-FG100 and
ROUTE-FG100 are both approved and active; a production order for
FG-CABINET-100 estimates to a total cost of $83.90/unit ($65.15 material +
$18.75 route), starts, and reports as finished, receiving a batch-tracked
finished-good quantity into inventory that can be traced back to the
specific raw-material batches consumed.

## Known caveats

- **MS Learn MCP was not available in this session** (authentication
  required) — the citations above were located via a web search scoped to
  `learn.microsoft.com` instead. Re-run the lookups through MS Learn MCP
  during Phase 2 execution to confirm the pages, exact field names, and any
  version-specific prerequisites before treating this as final.
- **Production order quantity is a runbook recommendation, not a sourced
  value** — no decision or workbook cell sizes the pilot batch. Confirm
  with Operations before Phase 2.
- **D-10 (pricing floor) is a known, deliberately unresolved gap**: Sales
  currently prices FG-CABINET-100 against `Item.StandardCost` ($83.90), not
  realized production cost, so a labor overrun on Assembly (the operation
  flagged in Session 2 as most exposed to hour overruns) could sell below
  actual cost without being caught until month-end margin analysis. This
  runbook does not configure a fix — per D-11, a pricing floor control is
  explicitly deferred until realized-cost reporting from this pilot is
  proven out. Surface this gap to the consultant reviewing the runbook; do
  not silently configure past it.
- **D-12 (batch reconciliation check)** — the standing check that no batch
  is consumed before its own receipt date is a monthly close procedure, not
  a one-time configuration step, and is out of scope for this single-item
  setup runbook. Flag it as a Phase 2 follow-up once transactions exist to
  check.
- This runbook only covers the single item, single route, single site
  scope explicitly bounded by D-01. Multi-vendor sourcing (raised and
  deferred for the lock assembly in Session 1) and multi-site/multi-currency
  are out of scope and not configured here.
