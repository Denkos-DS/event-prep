# event-prep

A Claude skill for planning group events — chalet weekends, camping trips,
festivals, birthdays, house parties, picnics, BBQs.

Group events fail in predictable ways. Quantities get multiplied by the wrong
number. Four trays don't fit in one oven. Breakfast gets bought for a morning
nobody is awake for. Vegan guests find nothing to eat. The budget balances on
paper because one line was an estimate nobody checked.

This encodes the arithmetic and the failure modes so they don't have to be
rediscovered every time.

---

## What it produces

Three documents, three audiences:

| Document | For | Contains |
|---|---|---|
| **Guest schedule** | everyone attending | one page — meals, times, dietary options |
| **Planning doc** | organiser and cooks | recipes, quantities, timelines, budget, logistics |
| **Shopping list** | whoever's in the store | itemised by store, tick-off, running total |

---

## Installing the skill

**In Claude:** package `skill/` as a `.skill` file and save it from the file card,
or upload `skill/SKILL.md` directly.

**In Claude Code:** copy `skill/` into your skills directory as `event-prep/`.

Once installed it triggers on its own — "we're renting a chalet for the weekend",
"how much chicken for 30 people", "what generator do we need".

---

## What's inside

```
skill/
├── SKILL.md              process, audit checklist, budget reconciliation
└── references/
    ├── quantities.md     per-person maths for food, drink and ice
    ├── logistics.md      venue survey, power, rentals, timelines, cold chain
    └── documents.md      the three-document set

examples/
└── chalet-weekend-aug-2026/    a real worked example, 33 guests, 4 days
```

---

## Some of what it knows

**Bone-in chicken is 30% bone.** Five kilos of drumsticks is 3.5 kg of meat — 135 g
each across 26 people, which is thin for a main. Establish which weight someone
means before scaling.

**The curfew cascade.** If music stops at 11 PM the party starts early, which moves
the main meal to mid-afternoon, which opens an eight-hour evening gap with drinking
in it and nothing planned to fill it. Leftovers stop being waste and become the
plan — say so, or people assume dinner was the end of it.

**Ice is what actually runs out.** 1–1.5 kg per person per day for cocktails. Spritz
over a full glass of ice consumes far more than anyone budgets for. Plan a
mid-event restock.

**Two air fryers will trip a breaker.** A 15 A circuit is 1,800 W and you should
plan on 1,440 W continuous. Two air fryers are ~3,000 W. Same maths sizes a
generator: sum running watts, add the largest single surge, add 25%.

**Don't marinate in citrus for two days.** Acid turns the surface of meat mealy and
chalky. Oil, aromatics and zest ahead; juice 45 minutes before cooking.

**Match the mixer to the spirit.** A 3:2 spritz means six litres of aperitif needs
exactly twelve 750 ml bottles of sparkling. Buy fewer and you're holding aperitif
you can't serve.

---

## Where this is going

See [`docs/PRODUCT.md`](docs/PRODUCT.md) for the function breakdown and build
plan. Short version: nine functions, of which two carry the value — the
**quantity engine** (the domain maths) and the **plan audit** (checking a plan
someone already wrote). Both currently live as prose the model applies; both
should become scripts, because arithmetic executed dozens of times across a long
session drifts, and a script makes that class of error structurally impossible.

Everything after that is optional.

---

## Origin

Built from an actual chalet weekend — 33 guests, four days, three cooked meals,
seven vegan, a $2,850 budget across five stores and three phone-ahead orders. The
worked example in `examples/` is that event, unedited.

## Licence

MIT
