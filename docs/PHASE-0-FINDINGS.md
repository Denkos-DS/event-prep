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
3. **The ice figure contradicts the skill's own table.** `quantities.md` says
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
