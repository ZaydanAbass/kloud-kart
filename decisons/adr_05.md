# ADR-005: Hire a Data Engineer first and an Analytics Engineer second

- **Status:** Accepted
- **Date:** 2026-11-12
- **Decision owner:** Zaydan Abass 
- **Client Signoff** **Nana Adjei*

## Context

The board has approved exactly two data hires this financial year. The company has an urgent need to protect the live site, fix the Monday slowdown, and address customer data compliance.

## Decision

We will hire a Data Engineer in Q1 to build the replica, nightly jobs, and deletion process. We will hire an Analytics Engineer in Q3 to build the dashboards and improve data visibility.

## Alternatives considered

### Alternative A: Hire a Senior Data Architect and a Junior Developer

- **How it would work:** The architect designs the system, and the junior builds it.
- **Why it is attractive:** The architect would provide strategic direction for the Nigeria pilot.
- **Why rejected:** We cannot afford a senior architect on the approved budget, and we do not need a planner—we need builders. We already know what needs to be built.

### Alternative B: Hire two Data Scientists

- **How it would work:** Both hires focus on machine learning and forecasting, as requested by investors.
- **Why it is attractive:** It delivers the "ML demand forecasting" wish list item.
- **Why rejected:** Data Scientists cannot build pipelines, fix the live database problem, or address data compliance. Without clean, reliable data first, their models would be useless.

## Consequences

- **What this buys:** The Q1 hire solves the most urgent problem (protecting the live site) immediately. The Q3 hire arrives once the foundation is stable, to build on top of it.
- **What this costs:** We will not have anyone dedicated to machine learning this year.
- **What it costs the client if I am wrong:** If the Analytics Engineer is hired too late, dashboard delivery slips into Q4, and Nana waits longer for her visual reports.
- **What would make me revisit this:** If the board insists on ML forecasting before the data foundation is ready, we would need to renegotiate the hiring budget to include a third person.

## What this survives

This survives budget cuts, because it prioritizes the hire that protects revenue (Data Engineer) over the hire that adds polish (Analytics Engineer).