# Transcript 1 — Runbook generation: weak vs. improved prompt

**Teaching material only — do not paste this into Copilot. This pattern
maps to Phase 1 of the lab, where you generate your own runbook.**

## Weak prompt

> "Make me a runbook for configuring this in Dynamics."

### What Copilot does with it

"This" has no referent, and neither MCP server is told what role to play.
A prompt like this tends to produce one of two unhelpful results: a
runbook scoped to whatever Copilot most recently looked at in the
conversation (which may not be the full item→BOM→route→production-order
chain at all), or — worse — Copilot reaching for the D365 ERP MCP
*immediately*, querying or even attempting to configure something in a live
environment before any plan has been reviewed. Neither is safe or useful:
one under-scopes the runbook, the other skips the approval-gate the whole
pattern depends on.

### Why this falls short

- **No definition of scope.** Which entities, in what order? Released
  products only, or the full chain through to a production order?
- **No tool assignment.** Nothing tells Copilot that MS Learn MCP is for
  *method* and D365 ERP MCP is for *live state* — so it may blend them, or
  skip straight to write calls against a live entity you didn't name.
- **No output contract.** Nothing says it needs checkboxes, MS Learn
  citations, or decision citations — so it typically won't have any of
  them.
- **No named source documents.** Without pointing at `discovery/` and
  `reference/`, Copilot has no configuration data to draw from at all and
  may invent plausible-looking numbers instead — or worse, quietly round
  or approximate a figure it should have pulled from the workbook.

## Improved prompt

> "Using the make-to-stock-configuration skill, generate a runbook named
> zava_make_to_stock_runbook.md covering released product setup, BOM,
> route, and a production order, in D365 dependency order. Pull the exact
> configuration values from reference/zava_item_bom_master.xlsx and
> reference/zava_route_rates.xlsx — use their computed totals, don't
> re-derive or approximate them. Pull the rationale for each from
> discovery/decision-log.md and discovery/workshop-transcripts.md. Look up
> each step's authoritative sequence using the MS Learn MCP server — don't
> configure anything yet, don't call the D365 ERP MCP server at all in this
> step. Every action gets an empty `[ ]` checkbox, an MS Learn citation, a
> source-workbook citation, and a decision ID citation."

### What Copilot does with it

Creates `zava_make_to_stock_runbook.md` with sections in dependency order,
each line citing all three sources:

```
## 3. Bill of materials
- [ ] Create BOM version for FG-CABINET-100: 4x RM-STEEL-001, 2x
      RM-HINGE-002, 1x RM-LOCK-003 (MS Learn: "Create a bill of
      materials" — data: zava_item_bom_master.xlsx!BOM — decision: D-05,
      Session 2)
```

No D365 ERP MCP calls are made — the runbook is generated entirely from
the skill, the reference workbooks, the discovery docs, and MS Learn
lookups.

### Why this works

- **Names the exact scope** (released product → BOM → route → production
  order) instead of "this."
- **Explicitly separates the two MCP servers' roles** and tells Copilot not
  to touch the live environment yet — this is what keeps Phase 1 a planning
  step instead of an accidental write.
- **Names the source documents, split by kind** — a reference workbook for
  values, a decision log for rationale — so the configuration data is
  real, exact, and traceable, not invented or approximated.
- **Specifies the citation contract**, matching the house rule in
  `copilot-instructions.md`, so the runbook is usable as a review artifact
  without reformatting.

## Takeaway

A runbook-generation prompt has to do three jobs at once: define scope,
assign each MCP server its role, and specify the output contract. Skip any
one of the three and you either get a shallow runbook, an unintentional
live write, or an unreviewable one.
