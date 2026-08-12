# event-prep — working notes

A Claude skill for planning group events. Read `README.md` for what it is and
`docs/PRODUCT.md` for the function breakdown and build plan.

## Current state — v0.1

Everything in `skill/` works and is installable as-is; it was used end-to-end to
plan a real 33-person weekend, which is what's in `examples/`.

```
skill/SKILL.md              process, plan-audit checklist, budget reconciliation
skill/references/           quantities, logistics, camping-and-festivals, documents
examples/                   the source event, unedited
docs/PRODUCT.md             nine functions, five phases — the brief
docs/PHASE-0-FINDINGS.md    what the skill got wrong — open until the event
```

`skill/scripts/quantities.py` exists and has 42 passing tests. `reconcile.py`
and `assets/` don't yet.

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

**Second fold-in, 12 August 2026** — findings 7–9. `documents.md` gained "Sizing
the set" and a dates-and-times sync rule; `quantities.md` gained "Every quantity
has a physical footprint"; `SKILL.md` gained "Open questions need a deadline" and
three more audit items. This one **amends a standing decision** — see the
document-set entry below.

**Camping and festival support, 12 August 2026.** New reference
`skill/references/camping-and-festivals.md` for events with no building, plus a
building/no-building branch on the venue survey, water in `quantities.md`, and a
"repeat event" section in `SKILL.md` covering standing menus and store rosters.
This is `PRODUCT.md` Phase 3 arriving early because it was asked for; it is prose
in `references/`, so it doesn't touch the gate either. **It is unvalidated** —
see "Testing the skill" below.

**Phase 1 started 12 August 2026, two days before the event — the gate was
lifted by decision, not because it expired.** The gate said scripts shouldn't be
written against guesses. That risk is real and was accepted knowingly. What
reduces it: `quantities.py` ports rates that are stable domain knowledge, the
test suite asserts the constants against `references/quantities.md` so the port
can't drift from its source, and the source event's real figures are pinned as
regression cases. **What the weekend can still change is the numbers, not the
shape** — so recalibration after the event is an edit to the constants and their
guard tests, not a rewrite.

`assets/` remains gated. It has no equivalent argument.

**Phase 1 — harden the skill.** In progress. Details below.

## Phase 1 progress

**Done — `skill/scripts/quantities.py`** (12 August 2026), with
`test_quantities.py` alongside it: 42 tests, standard library only, run with
`python test_quantities.py` from `skill/scripts/`.

Two design decisions in it worth not reversing:

- **Everything returns a `Range`, not a number.** The reference gives bands
  because the reality is a band. A midpoint invents precision the domain doesn't
  have, and an invented midpoint is exactly the figure that later gets defended
  as though it were measured.
- **The traps raise rather than guess.** `meat_kg(26, "chicken")` is a
  `ValueError`, not an assumption — bone-in and boneless are not the same
  quantity, so an ambiguous cut is a question, not a value.

The test suite has a third section worth understanding before editing it: it
pins the places where the **source event disagreed with the reference** — toum
was marginally under the two-occasion floor, breakfast was bought for three
mornings where the reference wanted 2.5, and the drinks poured well above the
party rate. Those assertions exist so a future edit can't quietly "fix" the
script to agree with the event. If the weekend proves the reference wrong,
change the reference and the guard test together, deliberately.

**Next — `skill/scripts/reconcile.py`.** Parse a costed list, verify line items
sum to section, store and header totals, report drift. The real spec already
exists: the private folder's `verify-docs.py` has been doing this against seven
live documents and has caught real errors repeatedly. Port it rather than
designing it. Per finding 9, it should treat dates and clock times as checkable
quantities alongside money.

## The original Phase 1 brief

Add `skill/scripts/quantities.py`. **Done — see above.**

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

Then `skill/assets/` — the HTML shells, content-free. Regenerating them per event
burns tokens and produces inconsistent results.

Note after finding 7: `examples/` holds only three of them. The cook's menu,
operational schedule and prep-assignment shells exist only in the private working
folder, so lifting shells from `examples/` alone reproduces the document set the
finding says was too small. Take the shapes from the live set, strip the content.

## Decisions already made — don't reopen these

- **Knowledge stays as prose in `references/`; arithmetic moves to `scripts/`.**
  Not everything should become code. The judgement calls — what to ask at intake,
  how to phrase a warning, when to push back on a request — belong in prose.
- **Personal data does not live in this repo.** Store profiles, venue details,
  group composition, the standing menu and the kit inventory live in the private
  working folder (and/or Claude project knowledge), never here. The repo stays
  portable and shareable; the local specifics don't. The skill carries the
  *mechanism* — see "The repeat event" in `SKILL.md` — and the private layer
  carries the contents.
- **~~The three-document set is fixed.~~ Amended 12 August 2026 by Phase 0
  finding 7.** Guest sheet, planning doc and shopping list are now the *floor*,
  not the set. The source event needed five documents and ran seven — a cook's
  menu, an operational schedule and prep cards are not optional once more than
  one person has a job. The rule is one document per (audience × moment of
  reading). This was reopened on evidence from real use, which is what Phase 0
  is for; the decision it replaces was made before the event existed. See
  "Sizing the set" in `skill/references/documents.md`.
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
event shape than the source one, ideally.

**Camping is the stress test, and `references/camping-and-festivals.md` is now
the answer to it — written 12 August 2026, but from reasoning rather than from a
trip.** The venue survey branches on whether there's a building; water, cooking
without an oven, cold without a fridge, the food-safety arc, fuel, shade,
sanitation and festival gate rules are all covered.

So the test is no longer "where does the skill assume a kitchen" — that sweep has
been done. It's now: **run a real camping or festival plan against the file and
find where the file is wrong.** Unvalidated figures to check first are the 6 L
per person per day water rate, the 8–10 people per burner figure, the 1 kg per
person per day of food ice, and the fuel table. `docs/PRODUCT.md` is right that a
speculative template is wrong in ways only a real trip reveals; the file says so
in its own header.
