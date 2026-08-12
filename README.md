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

Three core documents, one per audience:

| Document | For | Contains |
|---|---|---|
| **Guest schedule** | everyone attending | one page — meals, times, dietary options |
| **Planning doc** | organiser and cooks | recipes, quantities, timelines, budget, logistics |
| **Shopping list** | whoever's in the store | itemised by store, tick-off, running total |

Those three are the floor. Once more than one person has a job, the skill splits
further — a cook's menu, an operational schedule and one prep card per dish — on
the rule that a document serves one audience at one moment of reading. The source
event needed five.

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
    ├── quantities.md     per-person maths for food, drink, ice and water
    ├── logistics.md      sites, venue survey, power, rentals, timelines, cold chain
    ├── camping-and-festivals.md   events with no building
    └── documents.md      the core set, and how many an event actually needs

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

**Water is the heaviest thing on a camping list, and it's usually missing.** Six
litres per person per day covers drinking, cooking and washing up — for 33 people
over three days that's 594 litres and 594 kilograms, which is more than most
vehicles carry. Ask whether there's a tap on site before planning the menu, not
after.

**A camp menu arcs from perishable to shelf-stable.** Fresh fish and ground meat
on day one, whole cuts on day two, cured and vegetarian from day three — because
a cooler on day three is not a fridge. It lines up neatly with appetite decay:
the day people want to eat least is the day the cooking gets simplest.

**At a festival you get one trip from the car.** No second run, often no
re-entry, and glass banned at the gate. That single fact deletes the whole "safe
to run short on" category — every quantity has to be right on the day you pack.

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
