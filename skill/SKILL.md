---
name: event-prep
description: >-
  Plan and cost any multi-person gathering — chalet weekends, camping trips, festivals,
  birthdays, house parties, picnics, BBQs, retreats. Produces a guest schedule, a
  planning doc, and a store-by-store shopping list with running costs, plus cook and
  prep sheets once more than one person has a job. Use whenever someone is organising
  an event for a group, splitting costs among friends, scaling a recipe for a crowd,
  building a shopping list for a trip, working out how much meat or alcohol to buy,
  sizing rentals like sound systems and generators, working out power for an off-grid
  site, or feeding people somewhere with no kitchen, fridge or running water. Trigger
  on partial asks too: 'how much chicken for 30 people', 'we're renting a chalet',
  'what generator do we need', 'will my power station run this', 'how much water for a
  camping trip', 'what do we bring to a festival'. The per-person maths, multiplication
  traps, unit confusions and venue constraints are where these plans go wrong.
---

# Event Prep

Group events fail in predictable ways. Quantities get multiplied by the wrong
number. Someone discovers on the day that four trays don't fit in one oven.
Breakfast gets bought for a morning nobody is awake for. Vegan guests find
nothing to eat. The budget balances on paper because one line was an estimate
nobody checked.

This skill encodes the arithmetic and the failure modes so they don't have to be
rediscovered every time.

## The order of operations matters

Do not start with the menu. The menu is downstream of seven things that are cheap
to establish and expensive to get wrong:

1. **Headcount, with dietary sub-counts.** "30 people" is not enough. "26 regular,
   7 vegan" changes every meal.
2. **Arrival and departure times.** These determine which meals actually exist.
   An arrival at 7 PM means no arrival-day breakfast and a late first dinner. A
   departure morning means coffee and leftovers, not a cooked breakfast.
3. **Any curfew or hard time constraint.** This cascades further than people
   expect — see "The curfew cascade" below.
4. **Venue capacity — starting with whether there is a building at all.** A
   building silently supplies water, cold, cooking, power, shelter, toilets and
   waste disposal; remove it and every one of those becomes a line item with a
   weight, a cost and an owner. Then ovens, fridges, freezers, circuits, and
   whether there's a shop nearby. Ask before planning, not after.
5. **How many sites, and how far apart.** Never assume one. See "Ask how many
   places this happens in" below.
6. **What's already committed, and what's being collected.** See "Get the whole
   budget before costing anything" below.
7. **What they already own.** See "Ask what's in the garage" below.

Then budget, then menu, then quantities, then sourcing, then timeline, then
documents.

## Get the whole budget before costing anything

**Ask what is already committed and paid before pricing a single item.** Venue,
rentals, deposits, decoration — these land before anyone thinks about food, and
the person planning has usually stopped counting them because they're settled.

Skip this and the food budget gets mistaken for the event budget. In the source
event, food and drinks turned out to be under a quarter of the real cost, with
the venue alone more than half — and the per-person figure the group actually
owed had never been computed anywhere. Nothing was hidden. Nobody asked.

Establish three numbers at intake:

- **Committed** — what's already agreed or paid, itemised.
- **Collected** — per head, and how many people are actually paying. That is not
  always the headcount you cook for.
- **Estimated** — what you're about to plan.

Then reconcile, per the section below. A budget presented as "we have $2,850"
is almost always one line of a larger picture. Ask which line.

## Ask how many places this happens in

**Never assume the event happens in one location.** Ask directly: how many sites,
how far apart, is there vehicle access between them, where is there mains power
and where isn't there, and is the route between them lit.

A second site changes more than it looks like it should. In the source event the
music stage was a ten-minute walk from the accommodation, and every logistics
assumption in the plan had quietly depended on that not being true — consumables
have to be where the people are, fuel belongs at the site that burns it, load-in
becomes a scheduled task with names on it, a curfew triggers a migration rather
than a silence, and **the unlit route people walk back along at midnight is the
highest-consequence risk in most weekends.** Each site also needs its own owner.

**`logistics.md` "Multiple sites" carries the full consequences** — read it
whenever the answer is more than one.

Camping trips and festival camps are multi-site by definition — the stage, the
tents, the water and the cooking are in four different places. Treat single-site
as the special case, not the default.

## Ask what's in the garage

**Before sizing anything to rent or buy, ask what they already own.** Power
stations, solar panels, coolers, speakers, tents, folding tables, generators,
vehicles. The answer to a logistics problem is very often already in someone's
garage, and a process that only asks what to *rent* will never find it.

Two things make this worth asking properly:

- **Nobody can recite their own kit accurately from memory.** Expect it to arrive
  over several messages, incomplete, with model numbers missing. Ask for the
  numbers printed on the label, not the name people use for the thing.
- **It should persist between events.** This is the single most reusable piece of
  intake there is, and re-establishing it every time is the reason people don't
  bother.

## The repeat event

**Ask whether this group has done this before.** Most haven't planned their first
event — they have a menu they return to, stores they always use, and a headcount
that barely moves. When that's true, most of intake is already answered and
re-asking it is the main reason people stop using a process.

Three things are worth keeping between events. They are personal and local, so
they belong in project knowledge or a private working folder, **not in the skill
itself**:

- **A store roster.** What each store is genuinely good for, hours, phone number,
  lead time on large orders, volume-discount thresholds, and which items are
  non-substitutable there. Record the *routing* too — two stores on the same road
  are one trip, and that's worth more than any individual price.
- **A standing menu.** The dishes the group actually returns to, held as
  **per-person rates rather than batch totals**, each with the store it comes
  from and the equipment it needs. A rate scales to any headcount; a total from
  last year's event is wrong for this one and looks right.
- **A group profile.** Recurring headcount and dietary split, drinking rate, who
  cooks, who handles sound and rentals, what's collected per head.

The payoff compounds: intake for a known group at a known venue should be three
questions, not twelve, and the quantities stop being re-derived — which is where
they drift.

**But a standing menu does not survive a change of event shape, and this is the
trap.** The dishes are the most reusable thing the group has, so they get carried
to the next event unexamined. Port a chalet menu to a campsite and every oven
dish silently has no way to be cooked; port it to a festival and the glass ban
alone reroutes the drinks. **Re-check a standing menu against the new
constraints, dish by dish, before costing any of it** — see
`references/camping-and-festivals.md` for which dishes travel and which have no
equivalent without a building.

## Quantities that look interchangeable and aren't

This domain goes wrong in one characteristic way: **two quantities share a name
or a unit, mean different things, and the plan fails somewhere unrelated to
where the mistake was made.** Three known instances:

| | Reads as | Actually |
|---|---|---|
| Bone-in vs boneless meat | 5 kg of meat | 3.5 kg of meat |
| Watts vs watt-hours | "how much power" | what can be plugged in, vs for how long |
| mAh vs watt-hours | 136,000 of something | ~327 Wh usable |

**Whenever a number arrives with a unit that could mean two things, establish
which before scaling anything.** Say plainly which one you've assumed. There will
be more of these than the three above — treat an unfamiliar unit as a question,
not as a value.

## The curfew cascade

If music has to stop at 11 PM, the party starts early. If the party starts early,
the main meal moves to mid-afternoon. If the main meal is at 3 PM, there is now a
long gap between it and bedtime — with drinking in it — and nothing planned to
fill it.

That gap is where "we have too much food" turns into "we ran out at 10 PM."
Leftovers stop being waste and become the plan. Say so explicitly in the
documents, or people will assume dinner was the end of it.

Always ask about noise curfews, venue quiet hours, and any fixed timings.
Everything else bends around them.

## Open questions need a deadline, not just an answer

Every plan accumulates unresolved questions. **Sort them by when the answer stops
being actionable, not by what they are about** — otherwise the urgent ones hide
among the merely unresolved.

The mechanism is that questions get filed under their topic. A question about
Saturday feels like a Saturday problem. But if the only thing that answers it is
an order with two days' notice placed during business hours, it is a Wednesday
problem, and it will expire unanswered.

That is exactly what happened in the source event. *"Is the Saturday brunch a
sit-down meal?"* sat in a list headed **unresolved, not blocking**. The only fix
was more bread; the bread was a five-dozen order the plan itself called "not a
walk-in order"; the shop shut at 6 PM on the last day the call could be made. The
most time-critical item on the list was filed as the least.

**Give every open question a decide-by date, derived from the lead time of
whatever it gates:**

- supplier notice periods and minimum-order lead times
- store and supplier opening hours, on the specific days that remain
- collection and delivery slots
- rental pickup and return windows
- anything that must be bought in advance because it cannot be bought on the day

Then present the list **sorted by deadline, with the date on each line.** A
question with no deadline is genuinely not blocking. A question whose deadline is
today belongs at the top regardless of what it is about.

## Use the script for the arithmetic

**`scripts/quantities.py` does the per-person maths. Run it rather than working
the tables by hand** — that is the whole reason it exists. Across a long
planning session hand-applied arithmetic drifts, and on the source event costed
totals fell out of sync with their line items three times.

```bash
cd scripts && python -c "
import quantities as q
from datetime import datetime
print(q.meal_plan(datetime(2026,8,14,19,0), datetime(2026,8,17,10,0)))
print(q.meat_kg(26, 'grilled'))          # Range(4.68, 5.2) kg
print(q.mixer_bottles(6, 3, 2))          # 12 bottles, exact
print(q.water_litres(33, 3))             # 594 L — and 594 kg
"
```

What it covers: `meal_plan` (which meals exist, from arrival and departure
times), `meat_kg` with the bone-in correction, `edible_from_bone_in`,
`starch_kg`, `dips_kg`, `breakfast_items`, `drinks_count`, `mixer_bottles`
(ratio-locked cocktails), `ice_kg`, `water_litres`, `appetite_factor`.

Three things to know before using it:

- **Everything returns a `Range`, not a number**, because the reference gives
  bands. Report the band, or say explicitly where in it you have chosen to sit
  and why. Don't invent a midpoint.
- **Ambiguous input raises.** `meat_kg(26, "chicken")` is a `ValueError`, not a
  guess — bone-in and boneless are different quantities, so an unspecified cut
  is a question to ask, not a value to assume.
- **It has no event-spec schema.** Pass plain arguments. Dietary sub-counts are
  the caller's job: call it once per sub-group with that group's headcount.

`python test_quantities.py` from `scripts/` runs 53 tests if you have changed
anything. The rates come from `references/quantities.md`, which stays the source
of truth — if the script and the reference disagree, the reference wins and the
script is wrong.

## Reference files

Read these as needed rather than all at once:

- **`references/quantities.md`** — per-person maths for food and drink, the
  bone-in correction, multiplication rules, the appetite-decay curve, and the
  physical footprint every quantity carries (mass, what needs cold, which site).
  Read this whenever converting a headcount into amounts to buy.
- **`references/logistics.md`** — venue constraints, multi-site planning, power
  in all its forms (circuits, generators, battery stations, solar, vehicle-to-
  load), sound system sizing, cooking-timeline construction, transport and cold
  chain. Read this for anything involving the venue, rentals, electricity, or
  cook-day scheduling.
- **`references/camping-and-festivals.md`** — events with no building. Water as a
  line item, cooking without an oven, cold without a fridge, the food-safety arc
  across days, fuel, shade, sanitation, waste, and the festival gate rules that
  reroute a whole plan. Read this whenever there is no kitchen, alongside
  `logistics.md` rather than instead of it.
- **`references/documents.md`** — the core document set, how many documents an
  event actually needs, who each is for, and what goes in and stays out of each.
  Read this before producing deliverables.

## Auditing an existing plan

People often arrive with a menu or a list already drafted, sometimes generated
elsewhere. Audit it before building on it. These errors show up constantly:

- **Per-day quantities never multiplied.** A breakfast section says "amounts are
  per day" and the consolidated list carries the identical numbers. Check that
  every total is actually a total.
- **Meals that don't exist.** Breakfast on an arrival day when people arrive in
  the evening; a full cooked breakfast on a departure morning.
- **Bone-in weights treated as edible weight.** See `quantities.md`.
- **Condiments and aromatics missing entirely.** Salt, pepper, cooking oil,
  garlic and onion powder are in the recipes and absent from the list more often
  than not. Itemising the list line by line is what surfaces these — do it.
- **A dish's accompaniment invented rather than remembered.** If a plan says
  shish taouk with lemon wedges, or gives a dish an accompaniment that doesn't
  match its cuisine, ask. Taouk goes with toum.
- **Dietary sub-groups covered at dinner but not breakfast.** Easy to miss and
  very visible to the person affected.
- **A budget that is really one line of a larger budget.** If the plan costs food
  and drinks and never mentions the venue, it is not the event budget. Ask what
  else is committed before believing any per-person figure.
- **A rental priced but never specified.** A generator, a sound system or a van
  appearing as a cost with no capacity attached means nobody checked it carries
  the load.
- **Everything assumed to be in one place.** Check whether the party, the
  kitchen, the fridges and the beds are actually the same location. Plans rarely
  say when they aren't.
- **Sustained loads uncounted.** Anything that runs unattended — coolers,
  fridges, lighting — multiplied by the hours it actually runs, not the hours
  anyone is watching it.
- **Documents that disagree with each other.** Where a plan has more than one
  document, sum each independently and compare. They drift in both directions,
  and the drift is invisible to reading.
- **Documents that disagree on a day or a time.** Money gets reconciled; days,
  dates and clock times almost never do. A supplier phoned a day late is as
  broken as a total that doesn't add up.
- **Open questions with no deadline attached.** Anything unresolved that gates an
  order, a booking or a purchase with a lead time is due *before that lead time*,
  not "before the event." Re-sort the list by deadline and see what moves.
- **Quantities with no physical footprint.** A line that has a price but no mass,
  no cold requirement and no location is unfinished. Cheap, bulky, cold-chain
  items — drinks and ice above all — are where this bites.
- **A menu carried over from a different event shape.** Groups reuse their
  dishes, and the reuse is silent. Check every dish against the equipment that
  actually exists: an oven bake at a campsite and a glass bottle at a festival
  are both plans with no way to happen.
- **No water line on an event without a tap.** The heaviest item on the list, and
  the one most often missing altogether. See `quantities.md`.

Say what's wrong plainly and give the corrected number. Don't soften it.

## Budget reconciliation

When money is being collected from a group, reconcile before planning spend:

```
committed (venue, rentals, deposits already agreed)
+ estimated (food, drinks, supplies)
= required
- collected
= gap
```

Report the gap **per person**, because that's the number that determines whether
it's a problem. A $420 shortfall across 33 people is $13 each and nobody blinks.

Watch for **placeholder lines**. A budget that says "alcohol: $1,600" is usually
a guess, not a quote. Price the actual list — the slack in one over-estimated
line often covers the entire shortfall elsewhere. Ask whether a line is a
committed quote or an estimate before treating it as fixed.

## Working with someone else's requests

Group events have other stakeholders, and their requests arrive mid-plan. Apply
them as given. Where a request has a consequence they may not have seen, note it
once, briefly, and proceed:

- A quantity that's several times what the event uses — say so, list it as
  requested, and give the number that would actually cover it.
- A cut that removes something structural — apply it, but flag the one line worth
  keeping and why. ("Everything else" rarely means the coffee.)
- An ambiguous request where two readings have very different costs — flag it as
  a question to settle before the shop, not a decision to make for them.

When it's someone's birthday or their money, their preference wins. Note the
trade-off and move on.

## Producing the documents

Build the shopping list **itemised line by line with individual costs**, not as
category estimates. This is not presentation polish — the act of itemising is
what catches the missing salt, the forgotten cooking oil, the aromatics that live
in three recipes and no list.

After building or editing any costed list, **verify that the line items sum to
the stated section and store totals.** Edits accumulate and headers drift. Sum
them programmatically rather than by eye. State the reconciled figure.

Keep all documents in sync. When a quantity changes, it usually appears in three
places: the menu section, the shopping list, and the guest schedule. Update all
of them, then re-verify the totals.

## Tone in the documents

Write instructions for someone who is tired, slightly drunk, and cooking in an
unfamiliar kitchen. That means:

- Name the single step most likely to go wrong, and say so in those words.
- Assign people by name in advance rather than hoping someone volunteers.
- Explain why a rule exists when the reason isn't obvious, because people
  override rules they don't understand. ("Don't stir the potatoes" needs its
  reason attached or someone will stir them.)
- Put warnings on the item they concern, not in a general notes section.
