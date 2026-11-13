# Situation Diagnosis – Kloud Kart Data Strategy Engagement

All cost figures are estimates built from numbered assumptions (A1–A19). Every number in an arithmetic line carries the label of the assumption it comes from. Nana Adjei is unavailable, so each assumption records what was decided in her absence, and the questions that would confirm it are listed under each problem. Costs of lost sales are lost revenue, not lost profit, so they overstate the true loss.

## Assumptions Used Across the Diagnosis

Assumptions are numbered in the order they first appear. A8 and A9 are reused in Problems 2 and 3.

| # | Assumption | Value | Basis | Confidence |
| --- | --- | --- | --- | --- |
| A1 | Length of each slowdown | 1 hour | Assumed: lasts until Nana's export finishes and the site recovers | Low |
| A2 | Slowdowns per year | 15 | Brief says twice last month. I assume once a month normally, with a few bad months | Medium |
| A3 | Orders per hour at 7am on a Monday | 133 | Brief: ~4,000 orders a day, about 167 an hour. Assumed 20% below average at 7am, before the 8–11am peak | Medium |
| A4 | Share of customers who give up during a slowdown | 7.5% base (5–10% range, 20% high case) | Assumed. Nothing in the brief anchors it | **Lowest** |
| A5 | Average order value | GH₵65 | Assumed | Low |
| A6 | Nana's manual export step each Monday | 0.5 hours | Assumed: starting, waiting for and downloading the files. The spreadsheet rebuild is A18 | Low |
| A7 | Developer time per slowdown | 2 hours | Assumed: noticing it, diagnosing it, waiting it out, checking recovery | **Lowest of the labour assumptions** |
| A8 | Working weeks a year | 52 | Assumed: the process runs weekly | High |
| A9 | Cost of an hour of staff time | GH₵30 | Assumed | Low |
| A10 | Chance in a year of a severe incident in a peak period | 20% | Assumed. Nothing in the brief says how likely it is | **Lowest of the risk assumptions** |
| A11 | Length of a severe incident | 3 hours | Assumed: the 8–11am peak window | Low |
| A12 | Orders per hour at peak | 250 | Assumed: about 50% above the hourly average of ~167 | Medium |
| A13 | Share of customers unable to order in a severe incident | 50% | Assumed | Low |
| A14 | Customer deletion requests a year | 5 | Assumed | Low |
| A15 | Complaints to the Data Protection Commission a year | 1 | Assumed: one of the five requests | Low |
| A16 | Copies of the customer list in circulation | 10 | Assumed: the brief says nobody tracks them | **Lowest** |
| A17 | Time to find and delete one customer from one copy | 1 hour | Assumed | Low |
| A18 | Nana's Monday spreadsheet rebuild | 1 hour a week | Assumed | Low |
| A19 | Finance's weekly reconciliation to Nana's figure | 2 hours a week | Assumed | **Lowest of the Problem 3 assumptions** |

## Problem 1 – Data Exports Slow Down the Live Site

### A. Symptom

- The website slows down during Monday exports. Twice last month an export slowed the live site to a crawl.

### B. Root Causes

- The MySQL database holding customer and order data sits on the same machine as the live website.
- Exports run directly against the live production database, so reporting competes with customers for the same resources.
- There is no separate place to run reports, and a person starts the export by hand.

### C. Evidence

- "Nana Adjei, Head of Operations, rebuilds her Monday numbers from CSV exports." "Twice last month, a Monday export slowed the live site to a crawl."
- Scale from the brief: about 4,000 orders a day from a database of about 40 GB.

### D. Lifecycle Stages Affected

- Ingestion: the export is pulled by hand directly from the production database.
- Storage: one database on one machine serves both customers and reporting.
- Serving: Nana's Monday numbers depend on that manual export.

### E. Undercurrents

- Architecture: the website and the database share a single machine.
- Orchestration: Manual, a person starts the extraction, so it is not scheduled or controlled.
- DataOps: there is no monitored or performance-tested extraction process.

### F. Data-Quality Dimensions Affected

- Not a data quality problem. The data itself may be correct. The harm is to the availability of the website.

### G. Assumptions

Uses A1–A7 and A10–A13 from the table above.

### H. Cost

#### 1. Direct Loss (lost sales, not profit)

- Annual loss = A1 × A2 × A3 × A4 × A5.
- Base case (A4 = 7.5%): 1 × 15 × 133 × 0.075 × 65 = **GH₵9,726**.

| A4 (share who give up) | Annual lost sales |
| --- | --- |
| 5% | GH₵6,484 |
| 7.5% (base) | GH₵9,726 |
| 10% | GH₵12,968 |
| 20% (deliberately high case) | GH₵25,935 |

**Stress test.** I trust A4 least. Even if I am wrong about A4 by half (3.75%), the annual loss is 1 × 15 × 133 × 0.0375 × 65 = **GH₵4,863**, so the conclusion is still that the slowdowns cost only a few thousand cedis a year in lost sales. For context, 4,000 orders a day at GH₵65 (A5) is an estimated GH₵95 million of annual sales, so the direct loss is a tiny share of revenue. The case for fixing this is not lost orders. It is protecting the live shop as the company grows.

#### 2. Wasted Labour

Two kinds of staff time are lost because the export is manual and runs on the live database.

- Nana's manual export step = A6 × A8 × A9 = 0.5 × 52 × 30 = **GH₵780** a year.
- Developer time on slowdowns = A7 × A2 × A9 = 2 × 15 × 30 = **GH₵900** a year.
- Total wasted labour = 780 + 900 = **GH₵1,680** a year.

**Stress test.** I trust A7 least. If it is half as long (1 hour), the developer cost is 1 × 15 × 30 = GH₵450 and the total is **GH₵1,230**. Even then, the conclusion is that the labour cost is small, so labour is not what justifies the fix.

#### 3. Risk-Weighted Exposure

**What it means.** Direct loss and wasted labour are costs the company is already paying. Risk-weighted exposure covers something that has not happened yet: a worse event than the ones seen so far. It has two parts, how likely it is and how much it would cost. Multiplying them gives an expected annual cost:

expected exposure = probability × impact

**The scenario.** Last month's slowdowns happened at a quiet hour (7am on a Monday). The exposure is that the same export, or the same shared server, fails during a busy trading period, so the site becomes unusable rather than merely slow. The Nigeria pilot makes this more likely, because more orders will use the same server.

**Calculation.**

- Impact of one severe incident = A11 × A12 × A13 × A5 = 3 × 250 × 0.50 × 65 = **GH₵24,375** in lost sales.
- Expected annual exposure = A10 × impact = 0.20 × 24,375 = **GH₵4,875**.

**Stress test.** I trust A10 least. If the probability is half (10%), the expected exposure is 0.10 × 24,375 = **GH₵2,438**. The same result follows if A13 is halved to 25%.

**How to read it.** Even with a bold assumption, the expected cost is a few thousand cedis. This is a floor, not a full estimate, because it counts only lost sales during the incident. It leaves out three things that cannot be priced from the brief: customers who give up and do not return, the damage to the story told to investors who have just funded the company, and a failure on the launch day of the Nigeria pilot. The board should see the figure as the smallest believable cost, with those three risks on top.

### I. Questions to Ask Nana

- When the site crawled, how long did it last before it was normal again? (A1)
- Roughly how many orders come in on a Monday morning, compared with a normal hour? (A3)
- When the site was slow, did you see fewer orders come in? Did customers complain or leave? (A4)
- What does an average order come to? (A5)
- Has this happened before last month, and how often? (A2)
- How long does it take you to run the export each Monday, before you start the spreadsheet? (A6)
- Who fixes the site when it slows, and how long does that take? (A7)
- Has the site ever been unusable during a busy period, and what happened? (A10, A11, A13)

### J. Compliance and Board Risk

- **Compliance exposure: low.** A slow site is not a data-protection breach.
- **Risk to the board: medium.** The direct cost is small, but this is the company's revenue site, and it will carry more load as the Nigeria pilot and 15–20% yearly growth arrive.

## Problem 2 – Customer Data Is Copied With No Record of the Copies

### A. Symptom

- If a customer asks for deletion, nobody can say where their data is or whether it has been removed everywhere.
- The Data Protection Commission gives the company 90 days to respond to a complaint about a deleted customer's data.

### B. Root Causes

- Customer data is copied without monitoring or tracking.
- Marketing exports customer lists with no register of the copies, no owner, and no process for deleting a customer from every copy.

### C. Evidence

- "Marketing exports customer lists with no tracking of how many copies exist."
- "90 days to respond to a Data Protection Commission complaint about a deleted customer's data."
- The Marketing list holds about 300,000 people.

### D. Lifecycle Stages Affected

- Ingestion: customer data is exported without recording its owner, purpose or destination.
- Storage: an unknown number of files exist without access, retention or deletion controls.
- Transformation: segmenting and filtering the list can create further derived copies.
- Serving: customer data may continue to be used after the source record has been deleted.

### E. Undercurrents

- Security: personal data sits in uncontrolled files.
- Data management: no inventory, no owner, no deletion process.
- DataOps and orchestration: exports are made by hand and not logged.

### F. Data-Quality Dimensions Affected

- Not a data quality problem. The data may be perfectly correct. It is a control and compliance problem.

### G. Assumptions

Uses A8, A9 and A14–A17 from the table above.

### H. Cost

#### 1. Wasted Labour

Every deletion request needs a search of every copy, not just the ones that become complaints.

- Annual cost = A14 × A16 × A17 × A9 = 5 × 10 × 1 × 30 = **GH₵1,500** a year.

**Stress test.** I trust A16 least, because the brief says nobody tracks the copies. If there are half as many copies (5), the annual cost is 5 × 5 × 1 × 30 = **GH₵750**. The conclusion is the same either way: the staff time is small, and the real exposure is that the company cannot prove a customer's data was deleted.

#### 2. Direct Loss

- No penalty, legal fee or customer loss is confirmed in the brief.

#### 3. Risk-Weighted Exposure

The Data Protection Act, 2012 (Act 843) sets criminal penalties in penalty units. One penalty unit is GH₵12 (a published figure as at December 2023, so confirm the current value). The amounts below are **statutory maximums per offence, not expected costs**.

| Offence | Maximum fine | Maximum prison term |
| --- | --- | --- |
| Not complying with an enforcement or information notice | 150 units, about GH₵1,800 | 1 year |
| Failing to register with the Commission | 250 units, about GH₵3,000 | 2 years |
| Selling or offering to sell personal data | 2,500 units, about GH₵30,000 | 5 years |
| Any offence with no penalty specified | 5,000 units, about GH₵60,000 | 10 years |

**Sources and limits.** I took these figures from published legal summaries, not from the text of Act 843, and the summaries do not agree on every figure. One legal guide gives the general penalty as 500 units, not 5,000. These numbers must be checked against the Act itself or a lawyer before they go to the board. Which offence would apply to Kloud Kart, and the legal fees, are not known. The Data Protection Bill, 2025 is a draft and not current law.

**How to read it.** The fine ceilings are modest in cedis. The larger exposure is legal and reputational: the company could not prove a customer's data was deleted within 90 days, with 300,000 people on the list and a second country's law arriving with the Nigeria pilot. Each complaint repeats the problem, so the exposure recurs and grows with the list. Imprisonment is possible under the Act, up to the maxima in the table, and so is the damage of the Commission publicly naming Kloud Kart just after a funding round.

### I. Questions to Ask (Marketing)

- How many copies of the customer list exist, and where are they kept? (A16)
- How long would it take to find and delete one customer from one copy? (A17)
- How many deletion requests have arrived, and has any become a complaint? (A14, A15)
- Is Kloud Kart registered with the Data Protection Commission?

### J. Compliance and Board Risk

- **Compliance exposure: high.** A 90-day legal deadline, 300,000 people on the list, and no way to prove deletion.
- **Risk to the board: high.** Investors have just funded a company that told them it has a data strategy, and this is the one problem with legal consequences that cannot be capped.

## Problem 3 – Operations and Finance Have No Shared Definition of Revenue

### A. Symptom

- Nana's weekly revenue total and Finance's total for the same week do not match, and nobody can say which one is right.

### B. Root Causes

- There is no standardised definition of key metrics. Each person rebuilds the numbers by hand from their own export and counts "a sale" in their own way (placed, paid or shipped).
- There is no single place both draw from.

### C. Evidence

- "Nana rebuilds her Monday numbers from CSV exports; Finance's spreadsheet never matches hers."

### D. Lifecycle Stages Affected

- Ingestion: the two reports may use different extracts or run at different times.
- Transformation: different formulas, order statuses and dates give different totals.
- Serving: conflicting figures reach decision-makers.

### E. Undercurrents

- Data management: no common definition or owner for revenue.
- DataOps: spreadsheet logic is not tested, versioned or reconciled.
- Architecture: reporting depends on uncontrolled extracts instead of one shared source.

### F. Data-Quality Dimensions Affected

- Consistency: the same fact is held in two places that disagree. It is not accuracy, because both copies may be correct under their own definition.

### G. Assumptions

Uses A8, A9, A18 and A19 from the table above.

### H. Cost

#### 1. Wasted Labour

- Annual cost = (A18 + A19) × A8 × A9 = (1 + 2) × 52 × 30 = **GH₵4,680**.

**Stress test.** I trust A19 least. If Finance spends half as long (1 hour), the annual cost is (1 + 1) × 52 × 30 = **GH₵3,120**. Even then, the conclusion is that the staff time is small. The real cost is that the board cannot trust the revenue figure.

#### 2. Direct Loss

- None confirmed in the brief.

#### 3. Risk-Weighted Exposure

- Not priced. The board sees two revenue figures and cannot tell which to believe, which breeds mistrust of every other figure.

### I. Questions to Ask Nana

- How many hours does your Monday rebuild take? (A18)
- How long does Finance spend reconciling against your figure? (A19)
- Does this happen every week? (A8)
- What does an hour of staff time cost? (A9)
- What does "revenue" mean to each of you: placed, paid or shipped?

### J. Compliance and Board Risk

- **Compliance exposure: low.** No personal-data issue is involved.
- **Risk to the board: medium to high.** Revenue is the first number investors ask for, and two answers destroy trust.

## Summary of Estimated Costs

| Problem | Wasted Labour (GH₵ a year) | Direct Loss (GH₵ a year) | Risk-Weighted Exposure |
| --- | --- | --- | --- |
| 1. Exports slow the site | 1,680 (1,230 if A7 halved) | 9,726 base case (4,863 if A4 halved; 25,935 high case) | 4,875 expected (2,438 if A10 halved); a floor, not a full estimate |
| 2. Customer-data copies | 1,500 (750 if A16 halved) | Not confirmed | Statutory maximums of about GH₵1,800–60,000 per offence, plus unquantified legal fees. Figures to be verified against Act 843 |
| 3. No shared revenue definition | 4,680 (3,120 if A19 halved) | Not confirmed | Not quantified |

"Not quantified" means no cost was estimated. It does not mean the cost is zero.

## Ranking by Risk to the Board Conversation

1. **Customer-data copies (Problem 2).** This carries the compliance exposure: a 90-day legal deadline, 300,000 people on the list, and no way to prove deletion. The fine ceilings are modest, but the criminal penalties, the reputational damage and the Nigeria pilot make it the largest risk. It ranks first even though its cost is the least measured.
2. **No shared revenue definition (Problem 3).** The staff cost is small, but it attacks the number investors will ask for first. If two people give two answers, trust is lost, and that affects every figure the board is shown.
3. **Exports slow the site (Problem 1).** This is the best-measured problem and its cost is bounded: about GH₵9,700 of lost sales a year at the base case (up to about GH₵26,000 at the high case), GH₵1,700 of labour and about GH₵4,900 of expected exposure. It matters most as the company grows and the live shop must be protected, but it is the cheapest of the three to be wrong about.

Costs are not used alone to rank, because two of the three problems are only partly priced. A cost-only ranking would favour whichever problem happens to be easiest to measure.
