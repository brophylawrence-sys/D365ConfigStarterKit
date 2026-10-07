# Runbook template

Use this template to capture a reusable prompt sequence for a task you do
repeatedly — in this lab, or back on a real engagement. A runbook is not a
one-off answer; it's a checked, citable, re-runnable sequence someone else
(or you, next month) can follow without rediscovering the method.

Fill in one copy per recurring task. Keep the skeleton's structure — a
runbook a reviewer can't scan in 30 seconds won't get reviewed.

---

## Skeleton

```
# Runbook: <name of the recurring task>

**Purpose:** <one sentence — what question does this answer, and who asks it?>
**Trigger:** <when do you run this? e.g. "monthly close," "before each cutover," "on request">
**Source:** <which D365 tables/entities this draws from, and which tool
reaches them — D365 ERP MCP for live data, MS Learn MCP for the
authoritative method, a decision log for the business rationale>
**Skill(s) used:** <which skill file(s), if any, define the method>

## Steps

- [ ] <Action 1 — specific enough that someone else could run it>
      (source: <table/entity + key field, and which MCP tool>)
- [ ] <Action 2>
      (source: <table/entity + key field, and which MCP tool>)
- [ ] <Action 3>
      (source: <table/entity + key field, and which MCP tool>)

## Expected result / checkpoint

<What does "this ran correctly" look like? A number, a range, a named
record — something you can check without re-deriving the whole answer.>

## Known caveats

<Anything that trips this up — a manual step it doesn't cover, a data
quality issue it can't catch, an assumption baked into the method.>
```

---

## Filled example

```
# Runbook: Monthly production cost-variance flag check

**Purpose:** Catch production orders where actual cost diverged materially
from estimate before month-end close, so Finance isn't surprised by the
variance report.
**Trigger:** Run in the last week of each month, before the close checklist.
**Source:** ProdCostAmount / the costing sheet for each production order
closed this month, via the D365 ERP MCP server against the live legal
entity; the 10% materiality threshold is a client decision (see
discovery/decision-log.md, D-08), not a system default.
**Skill(s) used:** make-to-stock-configuration (for the D365 entity
relationships; the variance calculation itself is straightforward once the
costing sheet is pulled)

## Steps

- [x] Pull estimated vs. actual cost by cost element for every production
      order closed this month.
      (source: ProdCostAmount, via D365 ERP MCP)
- [x] Flag any cost element with |variance| > 10% of its estimate.
      (source: same pull; threshold per decision D-08)
- [x] For each flagged element, pull the underlying estimated vs. actual
      hours/rate from the route card, so the flag comes with a "why," not
      just a number.
      (source: ProdRouteTrans / route card, via D365 ERP MCP)
- [ ] Cross-check any flagged production order's finished-good output
      against sales lines already invoiced against it, in case a variance
      has already flowed into a margin that's been reported.
      (source: sales order lines for the item, via D365 ERP MCP)

## Expected result / checkpoint

A short table, one row per flagged production order, each with a named
driving cost element and a one-line explanation (e.g. "15 actual hours vs.
10 estimated, same rate — labor overrun, not a rate change"). Zero rows is
a valid, good result — it means nothing to flag this month.

## Known caveats

- The 10% threshold is a judgment call, not a system default — tune it per
  engagement based on what Finance considers material.
- This only catches variance already posted to the costing sheet; a
  production order still in process won't show up here yet.
```
