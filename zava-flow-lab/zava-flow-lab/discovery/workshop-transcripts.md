# Zava DIY — make-to-stock pilot: workshop transcripts

Three discovery sessions behind `decision-log.md`. Lightly edited for
length; decision points are marked inline as **[D-xx]** where the group
reached one.

**Attendees across sessions:**
Maria Chen — Operations Manager · Dev Patel — Production Supervisor ·
Sarah Lopez — Finance & Costing Lead · Tom Nguyen — Sales Ops ·
Facilitator — implementation consultant

---

## Session 1 — Scope, sourcing, and batch control

**Facilitator:** Let's keep this pilot small on purpose. One finished item,
one route, one site. Does that work for everyone?

**Maria:** Fine by me — we picked the tool cabinet because it's simple:
three components, nothing exotic. **[D-01]**

**Facilitator:** Sourcing — how many vendors feed the three raw materials
right now?

**Dev:** Just Acme for all three today. We *could* dual-source the lock
assembly, but that's a Phase 2 conversation. For the pilot let's keep it
single-vendor so we're not debugging a multi-vendor receipt flow and the
cost model at the same time. **[D-02]**

**Sarah:** Agreed, and while we're on sourcing — I want standard costing for
the raw materials in this pilot, not actual cost. If we let purchase price
variance and production variance both flow into the same number, nobody's
going to be able to tell which one moved. Standard cost the materials, let
any production variance show up cleanly on the labor and consumption side.
**[D-03]**

**Facilitator:** That also simplifies the reconciliation math nicely. Batch
tracking — full genealogy from PO to sale, or just lot-level for compliance?

**Maria:** Full genealogy. The retail contract requires we can trace any
sold unit back to which batch of steel and which batch of lock hardware
went into it, in case of a recall. **[D-04]**

**Dev:** While we're talking data quality — we had an incident last quarter
where a consumption transaction got keyed with a date before the receipt
even posted. Backdated entry, not a real issue, but it looked alarming in
the ledger until someone traced it.

**Sarah:** That's exactly the kind of thing I don't want to find by
accident during close. Can we build a standing check for it?

**Facilitator:** Yes — a batch's receipt date should never be later than a
transaction that consumes from it. We'll run that check every close cycle,
not just at go-live. **[D-12]**

**Maria:** Good. Let's move BOM and routing detail to the next session — we
need Dev's team to walk the shop floor process for that one.

---

## Session 2 — BOM, routing, and cost standards

**Facilitator:** Walk me through the bill of materials for the cabinet.

**Dev:** Four steel panels, two hinge kits, one lock assembly per unit.
That's locked from the engineering drawing — nothing to decide here, just
confirming it for the system. **[D-05]**

**Facilitator:** And the route — how many operations?

**Dev:** Two. Cutting, then Assembly. We don't subcontract either step for
this item — everything runs on our own equipment. **[D-06]**

**Facilitator:** Rates for each?

**Sarah:** Cutting is $45 an hour, that's the standard shop rate. Assembly
is $60 — it needs a certified operator for the lock and hinge fitting, so
it's a different labor grade.

**Dev:** And honestly, Assembly is where I'd expect variance if we ever see
it. It's the step most sensitive to operator experience — a newer operator
can run long on hours in a way that doesn't happen in Cutting, which is
mostly machine-paced.

**Facilitator:** Good to flag now — let's make sure the rate is set
correctly at **$45 for Cutting, $60 for Assembly** so that if Assembly does
run over, it's unambiguously an hours story and not a rate-setup error.
**[D-07]**

**Sarah:** On that — I want a materiality rule so we're not chasing every
half-percent variance every month. Anything under 10% of estimate at the
cost-element level, I don't want on the close checklist. Over 10%, I want
it named with a driver before we sign off.

**Facilitator:** 10% it is. **[D-08]** That's a clean threshold to encode —
we can flag it automatically per cost element rather than eyeballing the
costing sheet each month.

**Maria:** Let's bring in Tom for sales and pricing next session — I want
to make sure whatever cost number Finance is tracking here actually
connects to what Sales is quoting.

---

## Session 3 — Sales, pricing, and the margin gap

**Facilitator:** For the pilot, which customer are we selling this item to?

**Tom:** Just Riverside Builders right now — existing account, good
relationship, easy to get real sales data without touching new-customer
onboarding. **[D-09]**

**Facilitator:** Maria mentioned wanting the cost side and the pricing side
connected. Tom, walk me through how a sales price gets set today.

**Tom:** Honestly? We price against the item's standard cost in the system
— the $83.90 figure — plus a margin target. We don't see the actual
production cost from a specific batch or order. If a job ran over on labor,
that never reaches the sales desk.

**Sarah:** Which means if Assembly overruns the way Dev described last
session, and Sales prices off the standard cost like always, a line could
go out the door below what it actually cost us to make — and nobody would
know until we run margin analysis after the fact.

**Tom:** That's... actually already happened, hasn't it. We had one order
last cycle that Sales priced aggressively to win the account, using
standard cost as the floor. If actual cost came in above standard, that
order might be underwater right now and we wouldn't have flagged it.

**Facilitator:** Let's not try to fix the pricing process in this session —
that's a bigger conversation than the pilot scope. But let's be explicit:
this is a known gap, not a hypothetical one. **[D-10 — open]**

**Sarah:** Agreed. Once we've got realized-cost reporting running reliably
out of this pilot, *then* let's talk about a hard pricing floor check
before an order confirms. Not before — I don't want to design a control on
top of numbers we haven't proven out yet.

**Facilitator:** That sequencing makes sense. **[D-11 — deferred, pending
D-10]** So to summarize where we've landed: single item, single vendor,
single customer, standard-costed materials with two route operations,
10% variance materiality, full batch genealogy with a standing
receipt-before-consumption check, and one open pricing gap we're
deliberately not solving yet but want visible every time we look at margin.

**Maria:** That's the pilot. Let's get it into the system.
