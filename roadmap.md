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
