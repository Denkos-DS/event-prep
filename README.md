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
├── SKILL.md              intake order, audit checklist, budget reconciliation
└── references/
    ├── quantities.md     per-person maths for food, drink and ice
    ├── logistics.md      sites, venue survey, power, rentals, timelines, cold chain
    └── documents.md      the three-document set

examples/
└── chalet-weekend-aug-2026/    a real worked example, 33 guests, 4 days

docs/
├── PRODUCT.md            nine functions, five phases — the build plan
└── PHASE-0-FINDINGS.md   what using it for real turned up
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

**Power stations can't be ganged.** A 500 W station cannot start an 800 W
speaker, and buying five more doesn't change that — each device sees exactly one
battery. Generator sizing is additive; battery sizing isn't. Meanwhile a power
bank's 20,000 mAh is about 48 Wh once you convert from cell voltage and pay the
conversion losses.

**The loads that break a plan are the sustained ones.** An air fryer draws twenty
times what an electric cooler does and runs for fifteen minutes. Two coolers run
while everyone is asleep and eat most of a camping battery bank every day.

**Ask how far apart things are, in walking minutes.** If the party is ten minutes
from the kitchen, the drinks have to move, the fuel has to be at the stage, the
curfew starts a migration rather than a silence, and somebody has to light the
path people walk back along at midnight.

---

## Where this is going

See [`docs/PRODUCT.md`](docs/PRODUCT.md) for the function breakdown and build
plan. Short version: nine functions, of which two carry the value — the
**quantity engine** (the domain maths) and the **plan audit** (checking a plan
someone already wrote). Both currently live as prose the model applies; both
should become scripts, because arithmetic executed dozens of times across a long
session drifts, and a script makes that class of error structurally impossible.

Everything after that is optional.

[`docs/PHASE-0-FINDINGS.md`](docs/PHASE-0-FINDINGS.md) is the open log of what
using this for real has turned up. The gaps found so far are mostly in *intake*
rather than in the maths: it never asked what was already committed, never asked
whether the event happened in more than one place, and never asked what the
organiser already owned. Each of those changed a plan materially once asked.

---

## Contributing

Worked examples are the most useful thing you can send. A real event that broke
in a way the skill didn't predict is worth more than a correction to a table —
`docs/PHASE-0-FINDINGS.md` shows the shape. Open an issue or a PR with what went
wrong and what the plan had said instead.

Two conventions if you're adding to the skill itself: judgement stays as prose in
`references/`, and arithmetic belongs in scripts once they exist. Keep `SKILL.md`
under 500 lines and push detail into a reference file.

## Origin

Built from an actual chalet weekend — 33 guests, four days, three cooked meals,
seven vegan, five stores and three phone-ahead orders. The worked example in
`examples/` is that event, unedited — including the mistakes caught along the way.

## Licence

MIT
