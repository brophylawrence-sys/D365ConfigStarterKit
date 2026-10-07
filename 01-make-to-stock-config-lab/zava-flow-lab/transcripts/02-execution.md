# Transcript 2 — Executing the runbook: weak vs. improved prompt

**Teaching material only — do not paste this into Copilot. This pattern
maps to Phase 2 of the lab, where you execute your generated runbook
against a live legal entity.**

## Weak prompt

> "Go ahead and set this up in Dynamics."

### What Copilot does with it

Two things can go wrong at once here, and both are worse in a live system
than they'd be against a local file. First, no legal entity is named — if
one was mentioned earlier in a long conversation, Copilot may target it
correctly, but a fresh or long session has no reliable way to know which
of possibly several legal entities in the connected environment "this"
means, and picking the wrong one means real configuration in the wrong
place. Second, "go ahead" doesn't say to follow the runbook in order or to
verify each step before continuing — a response to this exact prompt in
testing skipped straight to creating the production order, because that
was the "goal," without the BOM version it depends on ever having been
created — and reported success anyway.

### Why this falls short

- **No named legal entity.** The single most consequential missing detail
  in a live-write prompt — a wrong guess here doesn't fail loudly, it
  configures the wrong place.
- **No instruction to follow the runbook in order.** Without it, Copilot
  may optimize for "get to the stated goal" over "complete every
  prerequisite," which is exactly backwards for a dependency chain.
- **No verification requirement.** Nothing says a step must be *confirmed*
  by the MCP server's response before the next one starts, or before its
  checkbox gets marked — so a silent partial failure can look identical to
  success in the runbook.

## Improved prompt

> "I just created a new legal entity in D365 named 'zav1'. Configure the
> make-to-stock item in this legal entity using
> zava_make_to_stock_runbook.md, via the D365 ERP MCP server. Work through
> the runbook in the order it's written — don't start a step until the one
> before it is confirmed in D365, not just attempted. After each action,
> check its box and record the actual record ID D365 returned next to it.
> If anything fails, stop and report the error instead of continuing."

### What Copilot does with it

Executes released product creation first, confirms each one exists via a
follow-up ERP MCP read before moving to the BOM step, and updates the
runbook line by line:

```
## 1. Released products
- [x] Create RM-STEEL-001, RM-HINGE-002, RM-LOCK-003, FG-CABINET-100 as
      released products in zav1. Confirmed via ERP MCP read: all four
      exist with status Released. (decision: D-01, D-05)
```

If a later step fails — say, the route can't be created because a work
center referenced in the runbook doesn't exist yet in `zav1` — Copilot
stops there, reports the exact error, and leaves that box and everything
after it unchecked, rather than skipping ahead.

### Why this works

- **Names the legal entity explicitly**, removing the single riskiest
  ambiguity in a live-write prompt.
- **States the ordering and verification rule** in the prompt itself,
  reinforcing (not just relying on) the skill file's sequencing rule — for
  something this consequential, redundancy is a feature.
- **Defines what "done" means** (a confirmed record, not an attempted call)
  before Copilot starts, so a checked box is trustworthy without you having
  to re-verify it yourself afterward.
- **States the failure behavior up front**, so a problem stops the run
  instead of getting silently worked around.

## Takeaway

Executing against a live system raises the cost of an ambiguous prompt
sharply compared to querying a local file — a guessed legal entity or a
skipped prerequisite doesn't just give you a wrong answer, it changes real
configuration. Name the target, state the ordering rule, and define what
counts as "done" before you say "go ahead."
