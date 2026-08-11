# event-prep — working notes

A Claude skill for planning group events. Read `README.md` for what it is and
`docs/PRODUCT.md` for the function breakdown and build plan.

## Current state — v0.1

Everything in `skill/` works and is installable as-is; it was used end-to-end to
plan a real 33-person weekend, which is what's in `examples/`.

```
skill/SKILL.md              process, plan-audit checklist, budget reconciliation
skill/references/           quantities, logistics, documents
examples/                   the source event, unedited
docs/PRODUCT.md             nine functions, five phases — the brief
docs/PHASE-0-FINDINGS.md    what the skill got wrong — open until the event
```

Nothing is under test. There are no scripts yet. That's the next phase.

## Where this sits in the plan

**Phase 0 — use it for real.** In progress. The source event runs mid-August 2026.
Do not start Phase 1 before it happens; the point of Phase 0 is finding out what
the skill gets wrong under real conditions, and building first risks hardening the
wrong things. Findings go in `docs/PHASE-0-FINDINGS.md` as they turn up — it's
already seeded with a pre-event pass over the example documents.

**Confirmed findings have been folded back into `skill/` as prose.** That is not
Phase 1 and doesn't breach the gate above — the gate exists so that *scripts*
aren't written against guesses, and knowledge belongs in `references/` by the
standing decision below. What went in: the intake questions the skill never
asked (committed costs, how many sites, what the organiser already owns), the
multi-site section in `logistics.md`, the rewritten power section covering
batteries, solar and V2L alongside generators, the unit-trap category in
`SKILL.md`, and six additions to the audit checklist.

`scripts/` and `assets/` remain gated on the event happening.

**Phase 1 — harden the skill.** The next work. Details below.

## First task when Phase 1 starts

Add `skill/scripts/quantities.py`.

**Why it exists:** the per-person maths currently lives as prose tables in
`skill/references/quantities.md` that the model applies by hand. That works, but
across a long planning session the arithmetic drifts. In building the source
event, costed totals fell out of sync with their line items at least three times —
each caught only by summing programmatically. A script converts that from
"usually caught" to "structurally impossible."

**Start minimal and testable.** A function taking headcount, cut type and meal
count, returning kilograms, with the bone-in correction applied. Grow from there.
Resist designing the full event-spec schema up front — `docs/PRODUCT.md` flags
that as an open question for a reason.

The domain rules live in `skill/references/quantities.md`. Port them; don't
reinvent them. The corrections that matter most:

- bone-in is ~30% bone, so bone-in and boneless are not interchangeable weights
- meal count comes from arrival/departure times, not day count
- cocktail ratios force mixer quantities (a 3:2 spritz means 6 L of aperitif
  needs exactly twelve 750 ml bottles)
- appetite drops 15–20% by day three

Then `skill/scripts/reconcile.py` — parse a costed list, verify line items sum to
section, store and header totals, report drift. Second priority but the same
motivation.

Then `skill/assets/` — the three HTML shells, content-free, lifted from
`examples/`. Regenerating them per event burns tokens and produces inconsistent
results.

## Decisions already made — don't reopen these

- **Knowledge stays as prose in `references/`; arithmetic moves to `scripts/`.**
  Not everything should become code. The judgement calls — what to ask at intake,
  how to phrase a warning, when to push back on a request — belong in prose.
- **Personal data does not live in this repo.** Store profiles, venue details and
  group composition go in Claude project knowledge. The repo stays portable and
  shareable; the local specifics don't.
- **The three-document set is fixed.** Guest sheet, planning doc, shopping list.
  See `skill/references/documents.md`.
- **Not building:** recipe database, payment splitting, vendor booking, price
  APIs, nutrition, RSVP. See the end of `docs/PRODUCT.md`.

## Conventions

- **Python 3, standard library only** unless there's a real reason otherwise. This
  runs inside a skill; dependencies are friction.
- **HTML uses system fonts and no CDN.** These documents get opened at venues with
  bad signal. Everything must work offline. Include print styles.
- **Always verify costed lists programmatically** after editing. Don't eyeball
  sums — this is the exact failure mode the repo exists to prevent.
- Keep `SKILL.md` under 500 lines. Push detail into `references/`.

## Testing the skill

Install `skill/` locally and run a fresh planning task against it — a different
event shape than the source one, ideally. Camping is a good stress test: no ovens,
no fridge, generator mandatory, water becomes a line item. Where the skill assumes
a kitchen, that's a gap worth writing down.
