---
name: event-prep
description: >-
  Plan and cost any multi-person gathering — chalet weekends, camping trips, festivals,
  birthdays, house parties, picnics, family outings, BBQs, retreats. Produces three
  documents: a guest-facing schedule, a full planning doc, and a store-by-store shopping
  list with running costs. Use this whenever the user is organising an event for a group,
  splitting costs among friends, scaling a recipe for a crowd, building a shopping list
  for a trip, working out how much meat or alcohol to buy, or arranging rentals like sound
  systems and generators. Trigger it even on partial asks such as 'how much chicken for 30
  people', 'we're renting a chalet for the weekend', 'help me plan the shopping', or 'what
  generator do we need' — the per-person maths, the multiplication traps, and the venue
  constraints are where these plans reliably go wrong.
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

Do not start with the menu. The menu is downstream of four things that are cheap
to establish and expensive to get wrong:

1. **Headcount, with dietary sub-counts.** "30 people" is not enough. "26 regular,
   7 vegan" changes every meal.
2. **Arrival and departure times.** These determine which meals actually exist.
   An arrival at 7 PM means no arrival-day breakfast and a late first dinner. A
   departure morning means coffee and leftovers, not a cooked breakfast.
3. **Any curfew or hard time constraint.** This cascades further than people
   expect — see "The curfew cascade" below.
4. **Venue capacity.** Ovens, fridges, freezers, electrical circuits, and whether
   there is a shop nearby. Ask before planning, not after.

Then budget, then menu, then quantities, then sourcing, then timeline, then
documents.

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

## Reference files

Read these as needed rather than all at once:

- **`references/quantities.md`** — per-person maths for food and drink, the
  bone-in correction, multiplication rules, and the appetite-decay curve. Read
  this whenever converting a headcount into amounts to buy.
- **`references/logistics.md`** — venue constraints, power and generator sizing,
  sound system sizing, cooking-timeline construction, transport and cold chain.
  Read this for anything involving the venue, rentals, or cook-day scheduling.
- **`references/documents.md`** — the three-document set, who each is for, and
  what goes in and stays out of each. Read this before producing deliverables.

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
