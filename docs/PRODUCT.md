# event-prep — Product Definition & Build Plan

Version 0.1 · August 2026

---

## Problem

Every group event repeats the same work from scratch, and the same errors. In
building a single 33-person chalet weekend, six substantive errors were caught in
a plan that already looked finished: breakfast quantities never multiplied for a
four-morning stay, 5 kg of bone-in chicken treated as 5 kg of meat, seven pantry
items that appeared in recipes and no shopping list, seven vegan guests with
nothing to eat at breakfast, a dish paired with the wrong condiment, and a $1,600
budget line that was really $800.

None of these are hard problems. All of them are invisible until someone checks,
and nobody checks, because the plan looks complete.

The cost is not usually money. It's the 8 PM realisation that there isn't enough
chicken, with 30 people already there.

## Who this is for

**Primary — the organiser.** One person who ends up owning food, budget and
logistics for a group of 20–40. Does this a few times a year. Isn't a caterer and
shouldn't have to become one.

**Secondary — the co-organisers.** Two or three people running specific lanes:
someone doing the alcohol run, someone on rentals, someone cooking. They need
their slice, not the whole plan.

**Tertiary — the guests.** Need one page. Will not read more.

## Goals

1. **No arithmetic errors reach the shop.** Every quantity traceable to a
   per-person figure and a meal count, and every costed list verified to sum.
2. **Constraints surface before the menu, not on the day.** Oven count, circuit
   load, fridge capacity, curfew, arrival time — all established during intake.
3. **Under an hour from "we're doing a thing" to three usable documents.**
4. **Each event makes the next one cheaper.** Stores, venues and group profiles
   accumulate rather than being re-established.

## Non-goals

- **Not a recipe app.** Recipes are an input. The value is scaling, sourcing and
  sequencing them.
- **Not a group-payments tool — but apportionment is in scope.** Narrowed
  14 August 2026 by Phase 0 finding 12. Splitwise exists and *settles*; it
  cannot express per-attendee-night buckets with per-bucket exclusions, which
  is exactly what a real group trip needs and what the organiser's own
  spreadsheet had been doing by hand. Computing **who owes what** is F4 at
  per-person resolution and belongs here (`scripts/split.py`). **Moving the
  money still doesn't.**
- **Not a booking platform.** It tells you what generator to rent, not where.
- **Not real-time collaboration.** Multiplayer editing is a v3 question at
  earliest, and probably never.
- **Not a general meal planner.** Weekday dinners for four are a different
  problem with different economics.

---

## Key functions

Nine functions. Two of them are the product; the rest are supporting.

| # | Function | Input → Output | Today | Next |
|---|---|---|---|---|
| **F1** | **Event intake** | vague description → structured spec | prose process in SKILL.md | works — formalise the spec schema |
| **F2** | **Quantity engine** | headcount + dietary split + menu + meal count → amounts | **`scripts/quantities.py`** (12 Aug 2026) | recalibrate constants after the event |
| **F3** | **Plan audit** | existing plan → ranked error list | checklist in SKILL.md | works — needs a test corpus |
| **F4** | **Budget reconciliation** | committed + estimated + collected → per-person gap | prose + **`scripts/split.py`** (14 Aug 2026) | fold the group-level gap into the same module |
| **F5** | **Sourcing router** | item list → store assignments, phone-ahead flags | routing principles | needs a store profile store |
| **F6** | **Timeline builder** | serve time + dish timings → backwards schedule | prose method | works |
| **F7** | **Logistics sizing** | appliance list → circuit load, generator size, sound rig | load tables | works |
| **F8** | **Document generation** | spec → guest sheet, planning doc, shopping list | prose specification | → HTML templates |
| **F9** | **Sync & verify** | edit → all documents consistent, totals reconciled | manual, ad-hoc scripts | **→ script** |

### The two that matter

**F2 and F3 are the product.** Everything else is competent formatting that a
capable person could do themselves.

**F2 (quantity engine)** is where the domain knowledge lives — the bone-in
correction, the ratio-matching on cocktails, ice consumption, appetite decay,
which meals actually exist given arrival and departure times. This is what
someone can't look up in five minutes.

**F3 (plan audit)** is the differentiated one. Nobody offers this. Most people
arrive with a plan already drafted — from a spreadsheet, a previous year, or
another AI — and want it checked rather than replaced. It's also the cheapest
entry point: no intake required, immediate visible value.

### Why F2 and F9 need to be code, not prose

Over a long planning conversation, costed lists drift. In building the source
event, section headers and store subtotals fell out of sync with their line items
at least three times, each caught only by summing programmatically. Prose tables
are fine for teaching a model the rules; they are not reliable for arithmetic
executed dozens of times across a session.

A script converts a class of error from "usually caught" to "structurally
impossible." That's the single highest-value change available.

---

## Build plan

Five phases. **Phases 0–2 are the real work.** Phase 3 is optional and Phase 4 is
a decision, not a commitment.

### Phase 0 — Use it (now, zero build)

Run the skill on the chalet weekend, end to end. Note every place it needed
correcting or produced something unusable.

This costs nothing and is the only source of honest signal. Building Phase 1
before doing this risks hardening the wrong things.

**Done when:** the weekend has happened and there's a list of what the skill got
wrong or missed.

### Phase 1 — Harden the skill (a weekend of work)

Add `scripts/` and `assets/`:

```
skill/
├── SKILL.md
├── references/          (existing)
├── scripts/
│   ├── quantities.py    spec JSON → amounts, with the corrections applied
│   └── reconcile.py     costed list → sum check, drift report
└── assets/
    ├── guest-sheet.html      the three shells, content-free
    ├── planning-doc.html
    └── shopping-list.html    filter + progress + running total
```

**`quantities.py`** — **built 12 August 2026**, with `test_quantities.py`
alongside it: 54 tests, standard library only. It owns the bone-in correction,
the meal-count logic, appetite decay, cocktail ratio-matching, ice and water.

It did **not** get the event-spec JSON schema sketched above, and that was
deliberate — the open question at the end of this document flagged the schema as
unresolved, so the module is a set of pure functions taking plain arguments
instead. A schema can wrap functions later; functions cannot easily be extracted
back out of a schema. Dietary sub-counts are still done by the caller passing the
relevant headcount, which is the part most likely to want revisiting.

Two decisions in it that shaped the rest: **every function returns a range rather
than a number**, because the reference gives bands and a midpoint invents
precision the domain doesn't have; and **ambiguous input raises rather than
guesses**, so an unspecified cut of meat is a `ValueError` rather than a silent
assumption about bone.

**`reconcile.py`** parses a costed list and verifies that line items sum to
section, store and header totals. Run after every edit. This is the fix for the
drift problem.

**`assets/`** are the HTML shells. Regenerating them from scratch each event
burns tokens and produces inconsistent results. Phase 0 finding 7 raised the
count: alongside the guest sheet, planning doc and shopping list, the source
event needed a cook's menu, an operational schedule and prep-assignment cards.

**Done when:** a second event can be planned without hand-writing verification
scripts.

### Phase 2 — Personal data layer (project knowledge, ongoing)

Lives in the Claude project, not the public repo — it's personal and local.

- **Store profiles** — what each carries, hours, phone, lead time, price posture,
  which items are non-substitutable there. A warehouse club, a bottle shop, a
  specialty grocer, a butcher and a bakery is the usual shape; the worked
  example in `examples/chalet-weekend-aug-2026` shows one such roster.
- **Group profiles** — recurring headcount, dietary split, who cooks, who handles
  sound and rentals, drinking rate.
- **Venue profiles** — chalets and sites used before: oven count, fridge capacity,
  circuit layout, distance to a shop, curfew.
- **Kit inventory** — what the organiser already owns: power stations, panels,
  coolers, vehicles. See finding 5.
- **Standing menu** — the dishes the group returns to, held as per-person rates.
  See "The repeat event" in `SKILL.md`.

This is what makes F5 (sourcing) work properly, and it's what makes the third
event dramatically faster than the first.

**Done when:** an intake for a known venue and group needs three questions
instead of twelve.

### Phase 3 — Event type templates (optional)

Different event shapes have different constraint profiles:

| Type | What changes | Status |
|---|---|---|
| Chalet weekend | the baseline — ovens, fridges, multiple buildings | **built**; event 14–17 Aug pending |
| Camping | no ovens, no fridge, water is a line item | **written**, unvalidated |
| Festival camp | multi-day, no power, extreme transport constraint | **written**, unvalidated |
| Day picnic | no cooking, everything transported cold and ready | not built |
| House party | kitchen unlimited, headcount uncertain, no accommodation | not built |

Each is a variant reference file under the same skill, in the pattern the
skill-creator guide recommends for multi-domain skills.

**Camping and festival camps were written together as
`skill/references/camping-and-festivals.md` (12 August 2026)**, on the reasoning
that they share one root cause — there is no building — and differ mainly in that
a festival adds a rules layer enforced at a gate. Splitting them would have
duplicated water, cold chain, fuel and cooking-without-an-oven twice over.

**They remain unvalidated.** The caution below still stands and the file says so
in its own header: correct it after the first real trip rather than trusting it.

**Only build the ones actually used.** A camping template written speculatively
will be wrong in ways only a real trip reveals.

### Phase 4 — Decide whether an app is warranted

**Do not start this before three real events.**

The question to answer is narrow: *what does an app do that a skill plus a shared
folder cannot?* Honest candidate answers:

- **Assignment tracking.** Co-organisers tick off their own lanes and everyone
  sees status. A static HTML file can't do this.
- **Persistence without Claude.** Guests and helpers who don't have accounts.
- **Return visits.** Opening last year's plan and forking it.

If those aren't compelling after three events, the app isn't warranted and the
skill is the finished product. That's an acceptable outcome, not a failure.

If it is warranted, the shape is a small web app with a shared event record, and
Claude behind it via the API for intake and document generation — not a rebuild
of the intelligence in application code.

---

## Success measures

Not analytics — this has one user for now. Direct observations:

**After the source event**
- How many things ran out? How many were thrown away?
- What did the plan miss that mattered on the day?
- Did anyone need to ask a question the guest sheet should have answered?

**After the second event (Phase 1 complete)**
- Time from start to three finished documents. Target: under an hour.
- Arithmetic errors surviving to the shop. Target: zero.
- Questions needed at intake. Fewer than the first time.

**After the third event (Phase 2 complete)**
- Intake questions for a known venue and group. Target: three or fewer.
- Was any of the store routing wrong?

---

## Open questions

**Deferred — was blocking Phase 1, and stopped being so**
- What's the spec schema? `quantities.py` shipped without one, deliberately: it
  takes plain arguments, so a schema can wrap it later, whereas functions cannot
  easily be extracted back out of a schema. Still open for `reconcile.py`, whose
  input format is a real design question rather than a deferrable one.

**Answered — was blocking Phase 2**
- Where does personal data live? **A private working folder** (`event-prep-private`,
  local git, no remote), and/or Claude project knowledge. Never this repo. This is
  now a standing decision in `CLAUDE.md`; the skill carries the mechanism and the
  private layer carries the contents.

**Non-blocking**
- Are the HTML documents the right format, or should the guest sheet be a PDF or
  an image? Cloud storage handles HTML poorly on phones, which is real friction
  at 33 recipients.
- Does the plan-audit function work on other people's plans, or is it tuned to
  the one plan it was built against? Needs a corpus of three or four real ones.
- Metric vs imperial, and multi-currency, if this ever goes beyond one user.

---

## What not to build

Recorded so they don't creep back in:

- A recipe database. Recipes are inputs.
- Payment splitting. Use Splitwise.
- Vendor booking or price APIs. Prices go stale; estimates are enough for
  planning, and the shopper sees the real price anyway.
- Nutrition tracking.
- Guest RSVP management.
- Anything that requires guests to have an account.
