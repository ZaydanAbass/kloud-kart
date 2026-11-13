# ADR-002: Refresh reports nightly and keep stock live, without building a real-time streaming system

- **Status:** Proposed
- **Date:** 2026-11-13
- **Decision owner:** Zaydan Abass
- **Client sign-off:** Decided in Nana Adjei's absence. To be confirmed by Nana.

## Context

Investors asked about "real-time analytics" and a board member wants "real-time dashboards" after seeing one at another company. The developer has mentioned streaming tools. The board has budgeted two data hires, not three, and the only developer works in PHP and MySQL.

The brief gives the rule that decides this: **stock counts can't go stale, because that is how the company oversells, and everything else can refresh daily.** Nana's reporting is also weekly, since the brief says she rebuilds her Monday numbers. Finance's rhythm is not in the brief, and I assume it is monthly.

So "real-time" splits into two different questions:

- **A customer is deciding whether to pay.** The answer must be right in seconds, because a stale answer means selling something Kloud Kart does not have.
- **Nana is deciding what to reorder or promote.** A day-old answer costs nothing.

I do not know exactly where stale stock counts arise today. I assume the website checks stock in the live database at checkout, and the stale counts come from exports and hand-built spreadsheets. This needs confirming with the developer (see the questions at the end).

## Decision

1. **The checkout stock check stays in the live database.** The website checks and reduces stock inside the same step that records the order. It never reads stock from the replica, from a copy, or from a batch job.
2. **All historical reporting refreshes once a night.** The nightly job reads from the read replica (ADR-001), and the shared sales table (ADR-003) is built from it.
3. **Nana's stock view reads the replica directly.** The dashboard queries the stock table on the replica, which is normally only seconds behind the live database. We set an alert if the replica falls more than 5 minutes behind (a figure I assumed). We do not build a separate 5-minute copy job.
4. **We do not build a streaming system now.** The answer to the investors is that Kloud Kart is real-time in the one place where seconds matter, checkout, and daily everywhere else.

## Alternatives considered

### Alternative A: Stream stock changes only (Kinesis or Kafka for stock, nightly batch for everything else)

- **How it would work:** Each sale publishes a "3 units of item 4471 sold" event to a stream. A small program reads each event and updates the stock number Nana sees, within seconds.
- **Why it is attractive:** It is the standard answer for stock that must be fresh, and it gives Nana a near-live number. It is also a smaller step than streaming everything.
- **Why rejected:** The customer-facing check is already live in the database, so a stream does not help the customer. For Nana's view, the replica gives nearly the same freshness with no new system. A stream adds a broker, a program that must run all day, and new ways to go wrong: an event delivered twice makes stock too low until someone corrects it, and a consumer outage leaves a stale number for the outage's length. A platform like this would also be likely to use up the time of one of the two hires.
- **Where it would win:** If stock changed in more than one system (for example a second warehouse or a sales channel that does not write to this MySQL database), or if the replica proves unable to stay within a few minutes under load.

### Alternative B: Keep a separate stock copy refreshed every 5 minutes

- **How it would work:** A scheduled job copies the stock table into a reporting table every 5 minutes, and Nana's dashboard reads that table.
- **Why it is attractive:** It uses tools the team already has, and it is what my first draft proposed.
- **Why rejected:** It adds a job to run and monitor, and it makes stock up to 5 minutes old, when the replica is usually seconds behind. The brief also says stock counts must not go stale, so building in a delay on purpose is the wrong direction.

### Alternative C: Run everything through both a batch and a streaming layer (Lambda), or through a stream alone (Kappa)

- **How it would work:** Lambda runs a nightly batch for accurate history and a streaming layer for fast results, and merges them. Kappa runs everything as one stream and replays it to rebuild history.
- **Why it is attractive:** It would give the board the "real-time dashboards" it asked for.
- **Why rejected:** Lambda means writing and testing the same logic twice, and the two layers can drift apart and give different numbers, which recreates the disagreement ADR-003 fixes. Kappa needs a streaming engine running 24 hours a day and someone with the skills to keep it running. Neither fits a team of two.

## Consequences

- **What this buys:** A system two new hires can run, with no new platform to operate. Customers are protected at checkout, and Nana's stock view is nearly live. The board gets a clear answer on "real-time" that rests on a decision, not a slogan.
- **What this costs:** The freshest history Nana sees is up to a day old. Her stock view is only as fresh as the replica, and a replica can fall behind under heavy load, so it must be monitored. Some of the board's wish for a live-looking dashboard is not met.
- **What it costs the client if I am wrong:**
  - If the website does not in fact check stock live at checkout, overselling continues and this ADR does not fix it. The cause would be in the website's code, and the fix would be there.
  - If the replica falls far behind during a busy period, Nana sees stock that is too high, and she may delay a reorder.
- **What would make me revisit this:**
  - Stock is updated by more than one system, or the Nigeria pilot adds a warehouse or channel outside this database.
  - The replica often exceeds the 5-minute alert, or the replication delay passes the 15-minute point set in ADR-001.
  - A decision appears that must be made in seconds and is not already made inside the live database, such as fraud checks.
  - Oversells continue after this change.

## What this survives

- **Investors pushing for real-time.** The ten-seconds-versus-a-day test gives a reason: only one decision, whether an item can be sold, must happen in seconds, and it already happens in the live database.
- **Growth of 15–20% a year.** About 4,000 orders a day grows to roughly 4,600–4,800 next year, which a nightly job and a replica handle without change.
- **The Nigeria pilot.** More orders and a second currency change the content of the reports, not how often they refresh. A second warehouse that sits outside this database would be the point to revisit.
- **Only two hires.** Nothing here needs an on-call engineer, because the nightly job can wait until morning to be fixed.

## Questions to settle before this is confirmed

- Does the website check and reduce stock inside the same step that records the order, or does it read a stock figure from somewhere else?
- Where did the stale stock counts that caused overselling come from, the site or Nana's spreadsheets?
- Does stock change anywhere other than this one MySQL database?
