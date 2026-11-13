# ADR-001: Run reporting on a read replica instead of the live database

- **Status:** Accepted
- **Date:** 2026-11-12
- **Decision owner:** Zaydan Abass 
- **Client Signoff** **Nana Adjei*

## Context

The live website and the MySQL database share one machine. Nana's Monday exports run directly against this live database and have slowed the shop to a crawl twice last month while customers were trying to buy. Finance has flinched at the cost of cloud warehouses.

## Decision

We will provision the pre-approved read replica and route all reporting and exports through it, keeping the live database exclusively for customer transactions.

## Alternatives considered

### Alternative A: Buy Snowflake or BigQuery (Cloud Data Warehouse)

- **How it would work:** Copy data continuously into a cloud warehouse and run all reports there.
- **Why it is attractive:** It is the modern industry standard, scales infinitely, and separates reporting from the live site.
- **Why rejected:** The database is only 40 GB and the company does 4,000 orders a day. The cost and complexity of a warehouse are not justified at this size, and the team has no experience running one.

### Alternative B: Schedule the export during off-peak hours only

- **How it would work:** Keep exporting from the live database, but only at 2 AM.
- **Why it is attractive:** Zero infrastructure cost. Requires no new server.
- **Why rejected:** It leaves the risk that an ad-hoc export, a crash, or a slow query still hits the live site. It treats the symptom (timing) rather than the cause (sharing resources).

## Consequences

- **What this buys:** The live website is completely protected from reporting workloads. Nana's numbers are accurate and no longer risk crashing the shop. In a nutshell, the cause of the slowdowns is removed.
- **What this costs:** We now have two databases to maintain. The replica must be monitored to ensure it stays in sync with the live site.
- **What it costs the client if I am wrong:** If the replica fails or falls behind, Nana's numbers will be stale, and we will have spent the setup time for nothing. The failure does not affect the live site though.
- **What would make me revisit this:** If Kloud Kart's data volume grows past 250 GB, if the Nigeria pilot introduces a need to join data across multiple different databases or if the process of replicating the databases typically takes longer than the estimated time (15 minutes)

## What this survives

This survives the pressure of growth. Even if order volume doubles, a dedicated replica for reporting will still protect the live checkout process. It also survives budget cuts, since it uses pre-approved infrastructure.
