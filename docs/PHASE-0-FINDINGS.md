# Phase 0 — findings log

Phase 0 is "use it for real and write down what it got wrong." That's this file.
It stays open until the source event has happened; what's in it becomes the
Phase 1 work list.

Seeded 11 August 2026, three days before the event, from a pass over the
documents in `examples/chalet-weekend-aug-2026/`.

---

## A. Document drift — found before the event

The three documents in `examples/` have fallen out of sync with each other. This
is the exact failure `skill/references/documents.md` warns about, happening
inside the repo's own worked example, which is the strongest possible argument
for `reconcile.py`.

**`planning-doc.md` is a stale earlier copy of `planning-doc.html`.** The HTML is
the current one. Three places prove it:

| | `planning-doc.html` | `planning-doc.md` |
|---|---|---|
| Fattoush pita sourcing | notes Costco pita is large-format and tears when fried crisp; routes fattoush pita to Adonis | note absent |
| "Never run short on" list | `baba ghannouj (Boucherie d'Orient)` once | lists `baba ghannouj` **twice** |
| Adonis line items | includes small Lebanese pita | not listed |

**`shopping-list.html` has diverged from both, in both directions.** Summed
programmatically:

| Store / section | Line items | Stated header | Delta |
|---|---|---|---|
| Costco → Produce | $120 | $126 | **−$6** |
| Costco → store total | $1,123 | $1,129 | **−$6** |
| Adonis → Fresh & dairy | $39 | $36 | **+$3** |
| Adonis → store total | $188 | $185 | **+$3** |
| SAQ Dépôt → store total | $696 | $626 | +$70 *(see below)* |
| Boucherie, Man'oushé, all other Costco sections | — | — | reconcile exactly |

- **Costco is short $6:** the shopping list drops `Limes, 6 — $6`, which
  `planning-doc.md` carries. Limes are separately covered by the 8-bag drinks
  line, so nobody goes without — but the header no longer matches its own items.
- **Adonis is over by $3:** the shopping list *adds* `Small Lebanese pita, 6 —
  for the fattoush — $3`, which is the correct fix and isn't in the markdown.
  The section header wasn't updated when it was added.
- **SAQ is not an error.** The header is the post-discount figure ($626 after the
  Dépôt 10%) and the items are list price ($696). But the page's running-total
  bar sums `data-p`, so filtering to SAQ in the store shows *"$0 of $696"*
  against a $626 header. Say which number is which on the page, or the shopper
  thinks they've overspent by $70 while standing at the till.

**Lesson for Phase 1:** every one of these was invisible to reading and obvious
to summing. `reconcile.py` should verify line items against section headers,
store headers *and* the budget-at-a-glance table, across all three documents at
once — not one document in isolation.

## B. Content defects — present in both HTML and markdown

These aren't drift; they're wrong in every copy.

1. **Phantom yogurt.** The chalet-logistics section reads *"13.5 kg of meat, 4 kg
   of yogurt and 7 kg of dips."* There is no yogurt anywhere on the shopping
   list — the nearest thing is 1 kg of labneh. Dips do total 7 kg (hummus 2.5 +
   baba ghannouj 2 + toum 2.5). Meat actually totals **14.5 kg**
   (5.5 chicken + 4 beef + 2.5 taouk + 2.5 kafta), not 13.5. Fridge planning is
   being done against numbers that don't match the list.
2. **Stale fruit note.** Breakfast was stripped back to manakish, croissants,
   eggs and jam — but the note about buying two-thirds of the avocados firm, and
   apples and pineapple keeping without fridge space, survived the cut. None of
   those three items are on any shopping list.
3. **Changelog contradicts the list.** "What Changed" says *"Toum 1 kg → 2 kg"*.
   The recipe, the shopping list and both HTML documents all say **2.5 kg**.

## C. Gaps — things the plan never covered

1. **Intake never asked what was already committed. This is the big one.**

   The plan presents its food-and-drinks budget as *the* budget. It isn't — the
   organiser was also carrying venue, sound-system, generator and decoration
   costs the whole time, and once they're added the food and drinks turn out to
   be **under a quarter of the real event**, with the venue alone more than half.
   The per-person figure the group actually owes was never computed anywhere.

   Nothing about this was hidden. The numbers existed; the skill never asked for
   them. `SKILL.md`'s order of operations says *"then budget, then menu"*, but
   the four intake questions it actually specifies are headcount, arrival and
   departure, curfew, and venue capacity. **None of them is "what's already
   committed, and what have you collected?"** So the plan organised itself around
   the one budget line that happened to be in the conversation.

   This is an **F1 (intake) defect that presents as an F4 (reconciliation)
   defect**, and it's the most generalisable thing Phase 0 has turned up — every
   group event has committed costs that land before anyone thinks about food.
   Fix intake and F4 has something to reconcile.

   **Add to intake, before the menu:** what's already committed and paid
   (venue, rentals, deposits, decoration), what's being collected per head, and
   how many people are actually paying — which is not always the same as the
   headcount you cook for.

2. **A rental line is not a rental spec.** Once the rentals surfaced, the
   generator turned out to be a price with no wattage attached. Sizing it by
   `logistics.md` — sound plus lighting, +25% headroom — lands around 3,500 W,
   but the same reference notes that adding cooking appliances pushes it past
   5,000 W. This plan has two air fryers in it. Whether they're meant to run off
   the generator changes the machine required, and nobody had asked.

   **Whenever a rental appears as a cost, the skill should ask what it has to
   carry.** Same for the decoration line, which arrived as a number with no
   items on any shopping list and no setup slot in a prep schedule whose Friday
   evening is already full.

3. **The venue survey assumes the event happens in one place. Fix this before
   anything else in `logistics.md`.**

   Asking what the generator had to carry produced an answer nobody had thought
   to volunteer: the music stage is **a ten-minute walk from the accommodation**.
   The plan was written throughout as though the party, the kitchen, the fridges
   and the beds were all the same location. They aren't, and almost every
   logistics assumption in the document quietly depends on that.

   What a second site actually changes, none of which was in the plan:

   - **The generator's risk moves from watts to run time.** Sizing was never the
     problem. Running dry mid-set with the fuel ten minutes away is — that's
     25 minutes of silence. Fuel belongs at the site that consumes it.
   - **Drinks and ice have to be where the people are.** Nobody walks twenty
     minutes for a beer, so the drinks migrate to coolers at the stage — and a
     cooler outdoors loses ice far faster than a fridge. A single ice figure
     computed for one location is wrong for two.
   - **Load-in becomes a scheduled task.** Speakers, a sub, stands, cables, a
     generator and fuel do not travel 500–800 m spontaneously.
   - **The curfew triggers a migration, not just silence.** `SKILL.md`'s curfew
     cascade already knows the post-party gap is dangerous; with a second site
     there's now a walk in front of it, and the late-night food has to be waiting
     at the other end before anyone leaves.
   - **Lighting the path between the two sites is a line item.** Thirty-three
     people walking unlit ground at 11 PM after several hours of drinking. It
     costs almost nothing in advance and cannot be bought on the night.
   - **Two sites need two owners**, and they can't be the same person.

   `logistics.md`'s venue survey asks about ovens, fridges, circuits, curfew and
   parking — all single-site questions. **Add: is this event on more than one
   site, how far apart, is there vehicle access between them, where is there
   mains power and where isn't there, and is the route between them lit?**

   This generalises further than the other findings. Two of the three event types
   `docs/PRODUCT.md` names as next — **festival camps and camping** — are
   multi-site by definition, with the stage, the tents, the water and the cooking
   in four different places. A survey that can only describe one location will be
   wrong about both of them in the same way it was wrong here.

4. **`logistics.md` knows about generators and nothing else that makes
   electricity.** Its power section is one appliance table and a
   generator-sizing rule. The organiser turned out to own a stack of camping
   power stations, folding solar panels, and access to two electric cars — none
   of which the skill has a single line about. It could not have helped, because
   it doesn't know they exist.

   Working the numbers surfaced a domain trap worth encoding verbatim, because
   it's the exact shape of the bone-in chicken correction: **watts and
   watt-hours look interchangeable and aren't.** A "500 W" power station is an
   *output* rating — what can be plugged in at once. Capacity is a separate
   number on the same label. Given only one of them, you cannot answer either
   "will this run?" or "for how long?", and the plan fails in a completely
   different place depending on which was meant.

   What the arithmetic actually showed, all of it generalisable:

   - **Output rating is a wall, not a shortage.** A 500 W station cannot start
     an 800 W speaker — it cuts out instantly. And **stations can't be ganged**;
     each device sees exactly one battery. Owning more of them changes nothing.
     Generator sizing is additive; battery sizing is not. That distinction is
     the whole difference and the reference doesn't make it.
   - **Peak decides what trips; average decides how long it lasts.** Powered
     speakers draw nameplate only on bass peaks — a rig with a 2,780 W peak
     averaged 820 W. Size the source on peak, size the battery on average, and
     say which number each table is quoting.
   - **Solar recharges, it doesn't supply.** 200 W of panel against an 820 W
     draw is a quarter of live demand. It's an overnight top-up.
   - **An EV with V2L is 50–100× everything else combined**, and outputs enough
     to run a whole party rig silently. Whether a given car supports it is the
     single highest-leverage question, and it is not something owners generally
     know about their own car.

   **Add a power-sources section covering batteries, solar and V2L, and add
   "what do you already own?" to intake.** The best answer to this event's power
   problem was sitting in the organiser's garage the entire time and no question
   in the skill would ever have found it.

   This matters most for exactly the event types `PRODUCT.md` names next.
   **Camping and festival camps are battery-and-solar events, not generator
   events** — the current reference is written for the one case that's least
   typical of where this is going.

   **Three unit traps, one shape.** Once the real inventory arrived, a third
   turned up: power banks are rated in **mAh at the internal cell voltage of
   3.7 V**, not at the 5 V they hand you. 136,000 mAh reads like an enormous
   number and is 503 Wh nominal — about 327 Wh after conversion losses, or
   roughly 22 phone charges. Enough for a camping group; nowhere near 33 people.

   | Trap | Looks like | Actually |
   |---|---|---|
   | Bone-in vs boneless | 5 kg of meat | 3.5 kg of meat |
   | W vs Wh | how much power | output ceiling vs how long it lasts |
   | mAh vs Wh | 136,000 of something | 327 Wh usable |

   These are the same defect three times: **two quantities that look
   interchangeable, aren't, and fail in different places depending on which was
   meant.** `quantities.md` already teaches the first one well. The pattern
   deserves naming as a category in the skill, because it's clearly how this
   domain goes wrong, and a fourth instance will turn up.

   **And the loads nobody counts are the sustained ones.** The organiser's two
   electric coolers draw ~36 W average between them — trivial-sounding, and
   **864 Wh a day, which is 87% of the entire battery bank**. Working the
   camping case: 964 Wh/day of draw against 585 Wh/day of solar is a 379 Wh
   daily deficit, so **two nights work and the third night is where the coolers
   die** — the night the food in them matters most. Nothing in the skill would
   have surfaced that, because it has no concept of a load that runs while
   everyone is asleep.

5. **The personal data layer needs a kit inventory, and it's an intake input.**

   `PRODUCT.md`'s Phase 2 names store, group and venue profiles. It doesn't name
   the one that turned out to matter here: **what equipment the organiser
   already owns.** Power stations, panels, coolers, vehicles — a standing
   register, read at the start of every event rather than re-established each
   time, and the natural home for the open questions too (which Anker model,
   whether the car does V2L).

   The generalisation: **the answer to a logistics problem is often already in
   someone's garage**, and a skill that only asks what to rent will never find
   it. The intake question is *"what do you already have?"*, and the reason it
   needs to be persistent rather than asked fresh each time is that nobody can
   recite their own kit accurately from memory — this inventory arrived over
   three messages and is still incomplete.

5. **The ice figure contradicts the skill's own table.** `quantities.md` says
   1–1.5 kg per person per day for a cocktail-heavy event. 33 guests over Friday
   and Saturday is **66–99 kg**. The plan buys 60 kg and says it "gets you
   through Friday and Saturday" — that's 0.9 kg/person/day, below the bottom of
   the range. Either the table is too high or the plan is short by 6–39 kg.
   **This is the one on the list most likely to bite on the night**, and it's
   cheap to settle: check whether the chalets have icemakers, and make the
   restock Saturday *morning* rather than "Saturday".

## D. Repo hygiene

- `LICENSE` said `Copyright (c) 2026 David`. Set to the full name for a public
  repo.
- No `.gitattributes`. Added — the HTML documents are CRLF on Windows and would
  churn the diff for anyone cloning on macOS or Linux.

---

## To fill in after the weekend

The questions `docs/PRODUCT.md` says to answer. Write the answers here while
they're fresh, not a week later.

- What ran out? What got thrown away?
- Was the ice enough? How much was left at the end of Saturday?
- Did the 3 PM main meal land, or did it slip? By how much?
- Did anyone set the 1:55 PM alarm? Did the handoff happen?
- Which quantities were visibly wrong in either direction?
- What question did a guest ask that the guest sheet should have answered?
- Did the two air fryers trip a breaker?
- Was anything bought that never got opened?
