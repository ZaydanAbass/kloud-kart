# ADR-003: Agree one written definition of a sale and build one shared sales table

- **Status:** Proposed
- **Date:** 2026-11-13
- **Decision owner:** Zaydan Abass
- **Client sign-off:** Decided in Nana Adjei's absence. To be confirmed by Nana and the Finance lead at the definition workshop.

## Context

Nana and Finance each rebuild the weekly revenue total by hand from their own CSV exports, and their numbers never match. Nobody can say which one is right. Two things are probably going on: each counts "a sale" in their own way (placed, paid or shipped), and each rebuilds the number separately. The brief does not say which, so the workshop has to find out.

The wasted staff time is small. At 3 hours a week between them (A18 and A19), 52 weeks (A8) and GH₵30 an hour (A9), it comes to about GH₵4,680 a year. The larger cost is trust. Revenue is the first figure investors ask for, and two answers to one question means the board cannot rely on any figure it is shown.

## Decision

We will do two things, in this order.

1. **Agree the definition.** Hold a one-hour workshop with Nana and the Finance lead to write down, on one page, what counts as "a sale" and which date it is counted on. My proposed starting point, assumed and not taken from the brief, is: *a sale is an order that has been paid, counted on the date of payment, with refunds subtracted on the date they are refunded.* If they disagree, the Finance lead owns the final wording, because Finance is accountable for the revenue figure. Nana can still see other views, such as orders placed but not yet paid, as separate labelled figures.
2. **Build one shared table.** The Data Engineer builds a single sales table, loaded nightly from the read replica (ADR-001) on the nightly schedule (ADR-002). Nana's and Finance's reports both read from it, and neither rebuilds the number from CSVs again.

The definition comes first because a table built on an undecided definition would just fix the disagreement in code.

## Alternatives considered

### Alternative A: Make Finance's spreadsheet the official one and have Nana use it

- **How it would work:** The two people stop keeping separate versions. Finance's spreadsheet becomes the company's revenue figure, and Nana reads from it.
- **Why it is attractive:** It costs nothing and could be done this week. Finance is already accountable for revenue, so there is a defensible owner.
- **Why rejected:** It ends the disagreement but not the cause. The spreadsheet is still hand-built from CSV exports, and the definition still lives in one person's formulas, so it would be lost if that person left. It also leaves Nana without the daily view she needs, which the dashboard later provides.
- **Where it would win:** If the workshop shows that the two totals differ only because Nana's version has a mistake, this is the cheapest fix and we should use it.

### Alternative B: Keep both spreadsheets and add an automatic comparison

- **How it would work:** A scheduled job compares the two totals each week and sends an alert when they differ by more than an agreed amount.
- **Why it is attractive:** It needs no change to how either person works, and it makes the gap visible straight away.
- **Why rejected:** It reports the disagreement without removing it. Two hand-built copies still exist, and someone still has to find out why they differ each time the alert fires.

### Alternative C: Buy an enterprise tool for managing business definitions

- **How it would work:** A master data management platform controls definitions across the whole company.
- **Why it is attractive:** It handles complicated cases and keeps an audit trail.
- **Why rejected:** I have not priced it, so I can't give a figure. Tools of this kind generally need a dedicated person to run them, and Kloud Kart has two data hires and one sales definition to agree. It is far bigger than the problem.

## Consequences

- **What this buys:** Nana and Finance look at the same revenue number, built the same way every week. The board gets a single figure with a written definition behind it. The definition is also written down, so it stops depending on one person's spreadsheet.
- **What this costs:** Both people give up their own formulas and rely on a shared table they did not build. The workshop takes an hour of two people's time, and the Data Engineer's time to build the table is part of the Q2 roadmap.
- **What it costs the client if I am wrong:** If the agreed definition is wrong, both departments will report the same wrong number with confidence, which is worse than today's disagreement, because nobody will think to question it. To reduce this, run the new table alongside both old spreadsheets for four weeks and explain every difference before the old ones are retired.
- **What would make me revisit this:**
  - Kloud Kart starts selling through other channels, such as wholesale, that need a different definition of revenue.
  - The Nigeria pilot adds a second currency and tax rules, so the definition must say which currency and whether tax is included.
  - The workshop shows that the two totals differ only because of an error, in which case Alternative A becomes the better choice.

## What this survives

- **Staff leaving.** If Nana or the Finance lead leaves, the definition lives on in a written page and a shared table, not in a personal spreadsheet.
- **Growth of 15–20% a year.** More orders change the volume the nightly job loads, not the definition, so the decision holds.
- **The Nigeria pilot.** The definition can be extended to a second currency without starting again, as long as it is revisited when the pilot launches.
- **Investor pressure.** When an investor asks for revenue, there is one number and a one-page definition behind it, which is a real answer to the question.
