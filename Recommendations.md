# Kloud Kart Data Strategy: Recommendation & Roadmap

## 1. Executive Summary

Kloud Kart has just closed a funding round and told investors it has a data strategy. However, the current reality is a shared database that crashes under reporting loads, conflicting revenue numbers between Operations and Finance, and unmanaged customer data that exposes the company to regulatory action. 

We do not recommend buying a data warehouse, building real-time dashboards, or implementing machine learning. Those are vanity projects that will fail given the two-hire constraint and 40 GB data size. 

Instead, we recommend a **Foundation-First strategy**. We will separate reporting from the live shop, establish a single definition of revenue, and secure customer data. This limits financial risk, resolves the Monday slowdowns, and gives Nana and the board numbers they can actually trust.

## 2. Core Recommendations & Justifications

### Recommendation 1: Separate Reporting from the Live Website
**What we will do:** Use the pre-approved second server to create a read-only copy (replica) of the database. All reporting and exports will run against this replica, never the live site.
**Why it fits Kloud Kart:** This permanently eliminates the Monday slowdown. It protects the ~4,000 daily orders and the GH₵95 million in annual sales the live site generates. It also respects the "money" constraint by using infrastructure we already have approval for, rather than buying a new cloud data warehouse.

### Recommendation 2: Implement Nightly Batch Processing (Reject Real-Time)
**What we will do:** Run data pipelines once a night, outside of trading hours. 
**Why it fits Kloud Kart:** The wish list asked for "real-time dashboards." We reject this. Nana makes operational decisions weekly; Finance makes them monthly. A day-late answer costs nothing. Real-time streaming (like Kafka) requires a dedicated specialist and a massive ongoing budget. By sticking to batch, we guarantee the two new hires can actually operate and maintain the system.

### Recommendation 3: Establish a Metadata or Data Dictionary
**What we will do:** Facilitate a workshop with Nana and Finance to define exactly what "a sale" means (e.g., placed, paid, or shipped). We will build one central data table that both departments use, ending the manual CSV rebuilds.
**Why it fits Kloud Kart:** Currently, Nana and Finance build their own numbers by hand, leading to conflicting reports. This destroys trust in the boardroom. A single source stops the GH₵4,680 a year wasted on manual reconciliation and ensures the board sees one, unified revenue figure.

### Recommendation 4: Formalize Customer Data Governance
**What we will do:** Create a Customer Data Register to track every copy of customer data, and establish a deletion process to meet the 90-day Data Protection Commission deadline.
**Why it fits Kloud Kart:** With 300,000 people on Marketing's list and no tracking, Kloud Kart is exposed to reputational damage and legal penalties. This recommendation does not just solve a tech problem; it protects the company's credibility with investors just as the Nigeria pilot begins.

## 3. Why the Wish List is Rejected
*   **"A data warehouse or lakehouse":** Overkill for a 40 GB database. A standard read replica handles this volume for a fraction of the cost.
*   **"Real-time dashboards":** Without a specific decision that must happen in seconds (other than stock availability), real-time is just expensive colour. Daily dashboards are enough.
*   **"ML for demand forecasting":** We cannot predict demand until we have clean, historical data flowing reliably. We must walk before we can run.
*   **"Just buy Snowflake":** Finance flinched for a reason. Enterprise tools require enterprise teams. We do not have one.

---

## 4. The 12-Month Roadmap

*Format: Quarterly, focused on business outcomes Nana can verify.*

### Q1: Protect the Shop and Map the Risk
*   **Protect (Rec 1):** Provision the read replica and schedule the nightly export.
*   **Comply (Rec 4):** Conduct a full audit of Marketing's customer lists to find out exactly where the 300,000 records live. Start drafting the Customer Data Register.
*   **Align (Rec 3):** Hold the "Revenue Definition" workshop.
*   **Nana’s Q1 Outcome:** *"The website never slows down on Mondays again, my numbers are ready without me running a manual export, and we know exactly where all our customer data is hiding."*
*   **Hire 1:** Data Engineer. *First 90 days:* Builds the replica and schedules the nightly data pulls.

### Q2: Secure the Data and Unify the Numbers
*   **Comply (Rec 4):** Launch the Customer Data Register officially. Build the automated process to delete a customer from every copy within the 90-day limit.
*   **Align (Rec 3):** Build the central "Sales" table so Nana and Finance pull from the exact same source.
*   **Nana’s Q2 Outcome:** *"If a customer asks to be deleted, we can prove we did it in days, not months. Finance and I now see the exact same revenue number."*

### Q3: Improve Visibility (The Dashboard)
*   **Deliver (Rec 2):** Build a simple, read-only dashboard that pulls from the new central tables.
*   **Deliver:** Automate the "Is this item in stock?" check using a 5-minute batch update (the one exception to daily updates).
*   **Nana’s Q3 Outcome:** *"I don't have to open Excel anymore. I can see our daily sales, top products, and current stock levels in one place."*
*   **Hire 2:** Analytics Engineer. *First 90 days:* Builds the dashboards and helps Marketing structure their data.

### Q4: Reliability and the Nigeria Pilot
*   **Scale:** Establish basic automated checks (e.g., alert us if daily sales drop by 50% or if duplicate orders spike).
*   **Scale:** Review the architecture for the Nigeria pilot (adding a second currency and tax rules).
*   **Nana’s Q4 Outcome:** *"If something breaks in the data, we know the same day. We are ready to handle the extra volume from Nigeria without the system crashing."*