# Documents

Three documents, three audiences. Keeping them separate is what makes them
usable — a guest scrolling past shopping costs to find dinner time will stop
reading, and a shopper filtering through recipes in a warehouse aisle will give
up on the list.

**Contents:** Guest one-pager · Planning document · Shopping list · Formats ·
Keeping them in sync

---

## 1. Guest one-pager

**For:** everyone attending. **Length:** one printed page.

Contains only what a guest can act on:

- Each day, each meal, with a time or an explicit "whenever"
- What's being served, in one or two lines
- Dietary options marked **inline on every meal**, not in a footnote
- Drinks available, listed once
- At most two closing instructions — the things guests can actually do

Leave out entirely: prices, quantities, shopping, cook times, who's cooking,
logistics, equipment.

Two things worth including that people forget:

- **What happens on arrival**, if the first meal is late. Guests who know food is
  out immediately don't raid the kitchen.
- **Which meals are self-serve and open-ended** versus fixed-time. "Breakfast:
  whenever" prevents a stream of questions.

Mark dietary options per meal rather than in one block. Seven vegan guests
reading that every single meal has something for them show up relaxed; the same
information in a footnote reads as an afterthought.

## 2. Planning document

**For:** the organiser and whoever is cooking. **Length:** as long as it needs.

Structure:

```
Header — budget at a glance, all budgets in one table, by stop
Prep schedule — what happens on each day before and during
Recipes — by day, scaled, with the vegan/dietary version alongside
Breakfast — as a spread with totals for the stay
Drinks — quantities, what it pours, the recipes
Consolidated shopping list — itemised, by store, with costs
Equipment — what to bring
Venue logistics — ovens, fridges, circuits, who does what
Top up on site — safe to run short on vs. no substitute nearby
What changed — a running log if you've revised someone's original plan
```

The header table should show **every budget line together**, split by stop. When
food and drinks are tracked separately, one table showing both is what reveals
that a single store carries half the weekend's spend.

Keep a **"what changed"** section when revising an inherited plan. It lets other
organisers see what moved and why without re-litigating it.

## 3. Shopping list

**For:** whoever is in the store. **Format:** interactive if possible.

This is the document that gets used under the worst conditions — one hand, bad
signal, a trolley, background noise. Design for that:

- **Filter by store.** In a warehouse aisle nothing else should be on screen.
- **Tick-off with a running total** that follows the filter, showing progress and
  spend for the current store.
- **Phone-ahead stops first**, with tappable numbers, addresses and closing times.
- **Warnings on the item they concern** — "ask for skin-on" beside the chicken,
  "ginger or citrus" beside the mixer, "restock mid-event" beside the ice.
- **Every line individually costed.** Not category estimates.

State whether tick state survives a page refresh. If it doesn't, say so.

## Formats

- **HTML** for anything that gets read on a phone or printed. Use system fonts
  only — venues have bad signal and web fonts won't load. Include print styles
  that drop navigation and avoid breaking sections across pages.
- **Markdown** for the planning document if it's being edited collaboratively.
- **PDF** for the guest page if it's going out over a file-sharing service —
  cloud storage often downloads HTML rather than previewing it, especially on
  phones. That's real friction when it's going to 30 people.

Shared cloud folders are the right home for these. The organiser edits one copy
and everyone sees the current version. Claude cannot write to a local drive —
the person downloads and files them.

## Keeping them in sync

A single quantity change usually touches three documents: the recipe or menu
section, the shopping list, and the guest schedule. Update all three in the same
pass.

**After any edit to a costed list, re-verify that line items sum to the stated
totals.** Section headers, store subtotals and the header table all drift as
changes accumulate. Sum them programmatically and state the reconciled figure.
Silent drift is worse than a visible overspend, because nobody catches it.
