# Phase 0 — findings log

Phase 0 is "use it for real and write down what it got wrong." That's this file.
It stayed open through the source event and now carries findings from later
events too — see section E. What's in it becomes the Phase 1 work list.

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

**B1.** **Phantom yogurt.** The chalet-logistics section reads *"13.5 kg of meat, 4 kg
   of yogurt and 7 kg of dips."* There is no yogurt anywhere on the shopping
   list — the nearest thing is 1 kg of labneh. Dips do total 7 kg (hummus 2.5 +
   baba ghannouj 2 + toum 2.5). Meat actually totals **14.5 kg**
   (5.5 chicken + 4 beef + 2.5 taouk + 2.5 kafta), not 13.5. Fridge planning is
   being done against numbers that don't match the list.
**B2.** **Stale fruit note.** Breakfast was stripped back to manakish, croissants,
   eggs and jam — but the note about buying two-thirds of the avocados firm, and
   apples and pineapple keeping without fridge space, survived the cut. None of
   those three items are on any shopping list.
**B3.** **Changelog contradicts the list.** "What Changed" says *"Toum 1 kg → 2 kg"*.
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
   time, and the natural home for the open questions too (which exact model a
   given battery is, whether the car does V2L). The register itself is personal
   data and lives in the private layer; only the shape of it belongs here.

   The generalisation: **the answer to a logistics problem is often already in
   someone's garage**, and a skill that only asks what to rent will never find
   it. The intake question is *"what do you already have?"*, and the reason it
   needs to be persistent rather than asked fresh each time is that nobody can
   recite their own kit accurately from memory — this inventory arrived over
   three messages and is still incomplete.

6. **The ice figure contradicts the skill's own table.** `quantities.md` says
   1–1.5 kg per person per day for a cocktail-heavy event. 33 guests over Friday
   and Saturday is **66–99 kg**. The plan buys 60 kg and says it "gets you
   through Friday and Saturday" — that's 0.9 kg/person/day, below the bottom of
   the range. Either the table is too high or the plan is short by 6–39 kg.
   **This is the one on the list most likely to bite on the night**, and it's
   cheap to settle: check whether the chalets have icemakers, and make the
   restock Saturday *morning* rather than "Saturday".

7. **Three documents wasn't enough. The set should be sized by how many people
   have jobs, not fixed at three.**

   `documents.md` fixes the set: guest sheet, planning doc, shopping list. This
   event ended up with seven live documents and genuinely needed at least five.
   The three that weren't in the set were a **cook's menu** (every dish scaled,
   with method and the warnings that matter mid-cook), an **operational
   schedule** (clock times, Wednesday calls through to Monday strike) and
   **prep-assignment cards** (one per dish, one name on each).

   The reason isn't that this event was unusually complex. It's that the three
   documents in the set are split by *topic*, and the split that actually
   matters is **when a document is read and by whom**. The planning doc is the
   organiser's reasoning, read once, before anything happens. The menu is read
   in a kitchen, with wet hands, three days later, by someone who was not in
   the planning conversation. The prep card is read by one person doing one
   thing. Collapsing those into "the planning doc" means a cook scrolls through
   budget reconciliation to find out how much salt goes in the marinade.

   The documents also **grew during planning rather than at the start** — the
   prep cards appeared once it became clear that "who is cooking Sunday" had no
   home. So the skill needs to be able to add a document mid-planning without
   re-deriving the others, which the current fixed set actively discourages.

   **Change `documents.md` from a fixed set of three to a rule:** one document
   per (audience × moment-of-reading). Guests before and during; the organiser
   before; the shopper in the store; the cook at the stove; each person with a
   job, the one job they have. For a small event several of those collapse into
   one sheet, and that's fine — but the collapse should be a decision, not the
   default.

8. **`quantities.md` has no concept of where a thing physically goes or what
   keeps it cold.**

   Ice, the power stations and 336 cans of drink all failed in the same shape:
   a number arrives, **the budget barely moves, and the real consequence lands
   somewhere the plan wasn't looking.** 336 cans is $354 — under 4% of the
   weekend — and roughly 143 kg competing for fridge shelves with 14.5 kg of
   meat, 7 kg of dips, 1 kg of labneh and 3 kg of salad. The binding constraint
   on this event turned out to be **cold, not money**, and there is no column
   anywhere in the skill for mass, volume, or what has to be refrigerated.

   The Thursday shop needs two vehicles. Nobody computed that; someone noticed
   it. 240 beers, 96 seltzer and 90 kg of ice do not fit in one car alongside
   luggage for the weekend — and that is a pure quantities-to-logistics
   consequence the reference cannot currently draw.

   `quantities.md` answers *how much to buy*. It never answers **where does it
   go, what keeps it cold, and who carries it** — and on this event those were
   the questions that actually bit.

   **Every quantity should carry a physical footprint alongside its cost:**
   approximate mass or volume, whether it needs refrigeration or freezing, and
   — once finding 3 above is in — which site it needs to be at. Then fridge
   capacity, cooler capacity and vehicle count become derivable instead of
   noticed.

9. **Open questions carry no deadline, so the time-critical ones hide among the
   rest.**

   Found on the last working day before the event, and the clearest single
   process defect Phase 0 has produced.

   The open-questions list was sorted into *blocking the Thursday shop*,
   *blocking Friday*, *unresolved from the party schedule*, and *not blocking*.
   The Saturday-brunch question — the 10 AM community brunch is a fixed
   sit-down, but breakfast is costed as a self-serve drift of ~20 manakish for
   33 people — sat in the third bucket, which reads as the least urgent.

   It was in fact **the most time-critical item on the list.** The only fix is
   more manakish; manakish are a five-dozen order that the documents themselves
   say "is not a walk-in order"; the shop closes at 6 PM on the one day the
   call can still be made. A question filed as *unresolved, not blocking* was
   silently gating an order with a lead time, and would have expired unanswered.

   **The mechanism:** questions get sorted by *what they are about* rather than
   by *when the answer stops being actionable*. A question about Saturday feels
   like a Saturday problem. It was a Wednesday problem, because the thing that
   answers it has two days of supplier notice in front of it.

   **Every open question needs a decide-by stamp derived from the lead time of
   whatever action it gates** — supplier notice, store hours, a collection slot,
   a rental return. The skill already holds the raw material to compute this:
   the shopping list knows which orders need notice, and the schedule knows when
   each supplier is open. Sort the open list by deadline, not by topic.

   **A second instance of the same shape, found the same night:**
   `shopping-list.html` told the shopper to phone Man'oushé **Thursday** while
   `food-schedule.html` put that call on **Wednesday**. Both documents were
   internally consistent and reconciled on every figure. `verify-docs.py`
   compares numbers across documents and cannot see a prose scheduling claim —
   yet a shopper following the wrong document gives one day's notice instead of
   two on the order the same document calls "not a walk-in order."

   **So `reconcile.py` should treat "when does this happen" as a checkable
   quantity, not prose.** Dates, days-of-week and clock times are exactly as
   cross-document-checkable as dollars, and this class of defect is invisible to
   both reading and summing.

## D. Repo hygiene

- `LICENSE` said `Copyright (c) 2026 David`. Set to the full name for a public
  repo.
- No `.gitattributes`. Added — the HTML documents are CRLF on Windows and would
  churn the diff for anyone cloning on macOS or Linux.

## E. From the second event — a three-night September camping trip

The first real plan run against `camping-and-festivals.md`, which is what
`CLAUDE.md`'s "Testing the skill" asks for. Logged during planning; field
results to follow after the trip. Dates and location stay in the private
folder — the finding does not need them.

10. **The reference has no concept of season, and season is a first-order
    variable.** Found in the first hour of planning, by arithmetic.

    `camping-and-festivals.md` was written in August against August figures,
    and several of them are silently seasonal: the 6 L water rate assumes
    summer heat, the solar yield assumes 4.5 peak sun hours, the cooler duty
    cycle assumes 28 °C ambient, and the food-safety arc assumes a cooler
    fighting summer. A September trip moves all of them at once — **and not
    in the same direction:**

    ```
    same kit, same 3-night trip     draw     solar    bank lasts
    high summer (as assumed)       964 Wh   585 Wh    2.6 days
    early autumn                   768 Wh   481 Wh    3.5 days
    ```

    Solar falls ~18%, but the coolers' duty cycle falls faster — so the
    September budget is *better*, and the kit inventory's own headline
    warning ("the third night is where the coolers die") stops being true
    the month after it was written. Meanwhile two new items appear that the
    file never mentions: **single-digit nights change the sleeping kit and
    push the menu toward hot food**, and the food-safety margin *relaxes*
    because an 18 °C day is kinder to a cooler than a 28 °C one.

    **The fix is not a table of months.** It's naming the seasonal inputs as
    inputs: ambient day temperature drives the cooler duty cycle and the
    ice rate; peak sun hours drive solar; night temperature drives kit and
    menu; and the water rate should say it assumes summer. A reference that
    states its assumed conditions can be corrected on site; one that hides
    them inside its constants gets trusted in the wrong month.

---

## F. From the audit — 13 August 2026

A full adversarial audit of both repos, a day before the source event. Twelve
agents across six dimensions; every finding independently verified before it was
accepted. It found one crashed gate, four published documents carrying stale
money, a float bug in `quantities.py`, and about thirty smaller drifts. All are
fixed. One finding generalises past its own fixes and is the reason the audit
was worth running.

11. **Enumerated coverage rots. Derive it instead.**

    Every tool in this project decides what to check by holding a hand-written
    list of things to check. Each list was correct when written. By the audit,
    all three had rotted, silently, in the same way:

    | List | What it enumerated | Rot found |
    |---|---|---|
    | `ALL_DOCS` | documents to sweep for stale strings | **omitted `budget.html`** — which was published to 33 people for weeks while carrying three figures from the verifier's own superseded list |
    | `STALE` (in the publisher) | strings that must not reach the shared folder | **all 22 entries dead**; not one appeared in any published document, while every genuinely stale string was absent from it |
    | `QTY` | quantities that must agree across documents | no entry for bottle counts, so a **halved drinks order** left one document pouring 166 spritz from 10 Aperol and 20 Prosecco against a live 6 and 12 |

    The pattern: **a hand-maintained list of what to check is itself an artefact
    that drifts, and nothing checks it.** It fails in the most dangerous
    direction available — silently, and toward *passing*. A rotted list reports
    "all clear" in exactly the voice a working one uses, so the tool keeps
    signalling safety while covering less and less. Worse, a second copy of a
    list (the publisher kept its own) cannot be kept in step with the first by
    discipline alone.

    **What actually fixed it, and the rule to carry forward:** derive coverage
    from the artefacts themselves rather than restating it.

    - The publisher's stale list is now *parsed out of* the verifier. One source,
      no second copy to fall behind.
    - The verifier now reads the publisher's file list and **fails if anything is
      published that it does not sweep** — a list that checks the list.
    - What can't be derived gets a guard: the deliberate coverage decisions are
      now assertions that fail when they stop being true.

    **This is `reconcile.py`'s most important design constraint, and it arrived
    before the script did.** The obvious port of `verify-docs.py` would inherit
    exactly this defect — a QTY dict, a SUPERSEDED list, a document roster, all
    hand-kept. Instead it should **walk the document set it is given**, extract
    every costed line and every repeated figure, and check them against each
    other. Enumerate only genuine exceptions, and make each one assert that it
    is still needed. The generalisation past this repo: **any checker whose
    coverage is a literal list will, given enough edits, quietly stop checking —
    so coverage belongs in code that reads the artefacts, not in a list beside
    them.**

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

From findings 7–9, added 12 August:

- **Which of the seven documents did anyone actually open?** Which was printed,
  which was read on a phone, and which never got used at all? That answer sizes
  the document set for every future event.
- **Did the fridges hold?** What ended up warm that should have been cold, and
  was the Saturday morning ice run enough?
- **Which open question expired unanswered** — and what did that cost on the
  day? Every one that did is evidence for the decide-by stamp.
- **Did the 10 AM Saturday brunch turn out to be a sit-down**, and was there
  enough food on the table at 10?
