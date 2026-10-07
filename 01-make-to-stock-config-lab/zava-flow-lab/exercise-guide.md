# Exercise guide — Zava DIY make-to-stock configuration lab

**You've just walked out of three Zava DIY make-to-stock discovery
workshops. The decision log and the transcripts are ready. Your next task
is Dynamics 365 configuration. You have Copilot, the D365 ERP MCP, and the
MS Learn MCP. Let's see how fast this goes.**

***

> **Before this session:** run the checks in `setup-guide.md`. You need a
> GitHub Copilot seat with agent mode, VS Code, the D365 ERP MCP and MS
> Learn MCP both configured, this workspace open as your folder root, and a
> Dynamics 365 Tier 2 or UDE environment with a legal entity you can
> configure. A Tier 1 or CHE environment is not sufficient.

## The scenario

You're a functional consultant on a Dynamics 365 Supply Chain Management
pilot for Zava DIY, a fictional home improvement retailer. The pilot is
narrow on purpose: one finished item (an assembled tool cabinet), built
from three raw materials, sourced from one vendor, produced on a
two-operation route, and sold to one customer. Three discovery workshops
produced two documents:

- **Decision log** (`discovery/decision-log.md`) — 12 numbered decisions
  covering scope, sourcing, costing method, BOM, routing, cost materiality,
  and pricing.
- **Workshop transcripts** (`discovery/workshop-transcripts.md`) — the
  three sessions those decisions came out of, with rationale and one
  deliberately open item.

The next step is D365 configuration. You'll do it in two phases, the same
shape every configuration cycle in this lab pattern follows: generate a
reviewable runbook first, then execute it.

***

## What you will do

### Phase 0 — Validate your skill file

Confirm `.github/skills/make-to-stock-configuration/skill.md` exists and
Copilot can read it — you did this already in `setup-guide.md`'s
verification step if you followed it. If you're working from a module this
skill doesn't cover, the pattern to build one is the same: capture the
method, the tool-usage rules, and the required sequence in a new skill
file before generating a runbook against it.

### Phase 1 — Generate the runbook

Open a new Copilot Chat session in agent mode. Enter **Prompt 1** below.

Copilot will use the make-to-stock-configuration skill, the MS Learn MCP
server, and both discovery documents to generate a structured runbook —
every configuration action in the correct D365 dependency order, with the
actual data to use, each item traceable back to a decision ID and (where
it applies) an MS Learn page.

> **The runbook is an approval artifact.** Before anything is created in
> D365, review it. If it looks right, move to Phase 2.

### Phase 2 — Execute the configuration

Enter **Prompt 2** below. Copilot will use the runbook and the D365 ERP MCP
to configure the item in your legal entity, checking off each action as it
completes and recording the D365 record ID it created.

***

## Prompts

### Prompt 1 — Create the runbook

```
We are running a make-to-stock manufacturing pilot for Zava DIY, a fictional
home improvement retailer. We ran three implementation workshops. The
resulting decision log and workshop transcripts are in discovery/
(decision-log.md and workshop-transcripts.md), and the exact configuration
data is in the reference/ folder (zava_item_bom_master.xlsx and
zava_route_rates.xlsx). These are added as attachments.

Your task: I want to configure one make-to-stock item end to end — released
product, bill of materials, route, and a production order ready to estimate,
start, and report as finished. Do not create the actual configuration in
Dynamics yet. Instead, use the make-to-stock-configuration skill to create a
new runbook named zava_make_to_stock_runbook.md that lists the configuration
actions to be undertaken, in the correct D365 dependency order, with the
actual configuration data to use.

For the sequence and setup steps, look up the authoritative steps using the
MS Learn MCP server. For the configuration data (item and components,
quantities, standard costs, route operations and rates, vendor, customer),
use reference/zava_item_bom_master.xlsx and reference/zava_route_rates.xlsx
— use their computed totals, not numbers retyped from the decision log's
prose. For the rationale behind each value, use discovery/decision-log.md
and discovery/workshop-transcripts.md. Only use the files mentioned here;
do not use any other files in the workspace, as they could be for a
different purpose.

When creating the runbook, reference the MS Learn page, the source workbook
and sheet, and the decision ID (and workshop session) for each action, so a
consultant validating the runbook knows where the step, the value, and the
rationale each came from. Every action should have an empty checkbox so we
can track progress in Phase 2.
```

**Checkpoint:** `zava_make_to_stock_runbook.md` exists, is ordered released
product(s) → vendor/customer → BOM → route → production order, every action
has an empty `[ ]` checkbox, and every action cites an MS Learn page (for
method), a `reference/*.xlsx` sheet (for the value), and a decision ID (for
rationale). The finished good's standard cost should appear as $83.90
(Material $65.15 + Route $18.75) — pulled from
`zava_item_bom_master.xlsx`, not restated from memory. The two open items,
D-10 and D-12, should appear as flagged notes rather than silently
configured around.

### Prompt 2 — Execute the configuration

```
I just created a new legal entity in D365 named "<your legal entity ID>".
Your task: configure the make-to-stock item in this legal entity using
zava_make_to_stock_runbook.md, via the D365 ERP MCP server. Work through the
runbook in order — don't skip ahead to a step whose prerequisite hasn't
actually succeeded yet. After each action, check its box and record the
actual D365 record ID or number returned next to it.
```

Replace `<your legal entity ID>` with the legal entity you set up for this
session.

**Checkpoint:** every runbook line is checked, each with a real D365
record ID next to it (item number, BOM version, route ID, production order
number) — not just a checkmark. If a step fails, the runbook should show
where execution stopped, not a silently skipped box further down.

***

## What good looks like

**After Phase 1** you should have `zava_make_to_stock_runbook.md` that:

- Covers all in-scope decisions from the decision log (D-01 through D-09;
  D-10 through D-12 noted as open items, not configured around).
- Has every action with an empty `[ ]` checkbox.
- References the specific decision ID/session, the reference workbook and
  sheet the value came from, and the MS Learn page for each action.
- Is ordered by D365 dependency: released product(s) → vendor/customer →
  bill of materials → route → production order.
- Uses the actual figures from `reference/*.xlsx` (item standard costs,
  BOM quantities, route hours and rates) rather than approximations.

**After Phase 2** you should have the item configured in your D365 legal
entity — released product, active BOM version, active route, and a
production order ready to run — with every runbook checkbox checked and a
real D365 record ID against each one.

***

## If something goes wrong — in this order

1. **Ask Copilot itself.** Paste the error. Ask *"what does this mean, and
   how do I fix it?"* Using Copilot to unstick itself is the muscle you're
   training today.
2. **Use your D365 knowledge.** Read what the ERP MCP actually returned.
   Identify what's wrong functionally — wrong sequence, missing
   prerequisite, incorrect entity — then instruct Copilot explicitly on how
   to work around it.
3. **Ask a neighbour or the facilitator.**

***

## Take this home

The runbook pattern here is reusable for any D365 module: bring your
discovery artifacts (workshops, decision logs, scope documents), generate a
module-specific runbook from them using the MS Learn MCP server for method
and your discovery docs for data, review and approve the runbook, then
execute it via the D365 ERP MCP — checking off each line against a real
record it created. The topic changes every time; the shape of the work
doesn't.

If you want to build this pattern for a module that doesn't have a skill
file yet, the method is the same one this lab's skill file demonstrates:
capture the tool-usage rules, the entity dependency order, and the
citation/output format once, in `.github/skills/<module>/skill.md`, then
point Phase 1 at it.
