# Kloud Kart: Data Strategy Engagement

Recommendation for Kloud Kart, an online shop with about 4,000 orders a day, a 40 GB database, and a board that has approved two data hires. Nana Adjei, Head of Operations, was unavailable for questions, so every figure is an estimate built on stated assumptions, and each assumption is marked for her to confirm.

## The problem in one paragraph

Reports run on the same machine and database as the live shop, so a Monday export has slowed the site twice. Nana and Finance each rebuild revenue by hand and never get the same number. Marketing copies a 300,000-person customer list with no record of the copies, so Kloud Kart cannot prove a deletion within the 90-day deadline.

## What we found (ranked by risk to the board)

| # | Root cause | Estimated cost | Why it ranks here |
|---|---|---|---|
| 1 | Customer data is copied with no record of the copies | Fines up to about GH₵60,000, plus legal costs | Legal exposure and no proof of deletion |
| 2 | No shared definition of revenue | About GH₵4,700 a year in staff time | The board cannot trust its revenue figure |
| 3 | Reports run on the live shop database | About GH₵9,700 lost sales and GH₵1,700 labour a year | Real but bounded; matters more as the company grows |

Lost sales are revenue, not profit, so they overstate the true loss.

## What we recommend

1. Run reports from a separate read-only copy of the database (ADR-001).
2. Refresh reports nightly and keep stock live at checkout, with no streaming system (ADR-002).
3. Agree one written definition of a sale, then build one shared sales table (ADR-003).
4. Keep a register of every copy of customer data and a log of every deletion (ADR-004).
5. Hire a Data Engineer first (Q1) and an Analytics Engineer second (Q3) (ADR-005).

## What we chose not to build

Real-time streaming for everything, a large cloud data warehouse, and machine-learning forecasts. The answer to investors is real-time where seconds matter (checkout) and daily everywhere else.

## Improvement beyond the brief

**A simple website to track stock** a freshness badge showing how old Nana's stock numbers are, and a list of oversold items so she can contact customers first. It costs no extra software.

## Repository contents

| File | What it is |
|---|---|
| `situation_diagnosis.md` | Root causes, costs, assumptions and ranking |
| `Recommendations.md` | The recommendation and its reasoning |
| `roadmap` | Quarterly roadmap, each quarter ending in something Nana can check |
| `decisons/` | Architecture decision records (ADR-001 to ADR-005) |
| `improvement.md` | Stock Watch |
| `Kloud_Kart_Board_Deck (3).pptx` | Board presentation |

## What could not be verified

Slowdown length and customer drop-off, average order value, how many copies of the customer list exist, whether Kloud Kart is registered with the Data Protection Commission, what "a sale" means to Nana versus Finance, which penalty applies, and Nigeria's data-protection rules. A lawyer should review the customer-data plan before it is relied on.

## Author

Zaydan Abass
