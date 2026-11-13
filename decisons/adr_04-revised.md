# ADR-004: Keep a register of every copy of customer data and a log of every deletion

- **Status:** Proposed
- **Date:** 2026-11-13
- **Decision owner:** Zaydan Abass
- **Client sign-off:** Decided in Nana Adjei's absence. To be confirmed by Nana, and reviewed by a lawyer before it is relied on.

## Context

Marketing exports customer lists with no tracking of how many copies exist. About 300,000 people are on the list. Kloud Kart has 90 days to respond to a Data Protection Commission complaint about a deleted customer's data. Today nobody can say where a customer's data is, and nobody could prove that it was deleted.

This is the top-ranked risk in the diagnosis (Problem 2). The staff time involved is small: about GH₵1,500 a year to search every copy for each deletion request (A14, A16, A17 and A9). The exposure that matters is that the company could not prove deletion. The Data Protection Act sets maximum fines of about GH₵1,800 to GH₵60,000 per offence, with prison terms possible, and these figures still need checking against the Act itself.

Two facts make this harder than it looks. First, the brief does not say what tools Marketing uses, so I do not know how the copies are made. Second, the Nigeria pilot brings a second country's data-protection law, which I have not checked.

## Decision

We will do three things, using documents and rules rather than new software.

1. **A register of copies.** A shared, access-controlled spreadsheet with one row for every copy of customer data: where it is kept, who owns it, why it was made, who has access, the date it was made, a delete-by date, and the steps to remove one customer from it. The register lists copies, not people, so it holds no personal data itself. It is kept separate from the reporting database.
2. **A log of deletions.** For each deletion request: the date received, who handled it, which copies were checked, the date each copy was cleaned, and a screenshot or note as evidence. The 90-day clock is started in the log on the day the request arrives. This is what lets Kloud Kart show that a deletion happened, which a register alone cannot do.
3. **A rule for exports.** No customer list is exported unless a row for it is added to the register first, with a delete-by date. Copies that are no longer needed are removed on that date.

Before this is relied on, the Data Engineer and Marketing run one practice deletion on a test record, from request to a completed log entry, and time it. This tests the process and replaces my assumed 1 hour per copy (A17) with a real figure.

I propose that the head of Marketing owns the register, because Marketing makes the copies, and that the Data Engineer checks it each quarter. This is an assumption and needs confirming.

## Alternatives considered

### Alternative A: Buy an enterprise data catalogue that finds personal data automatically

- **How it would work:** A tool scans company systems and maps where personal data lives.
- **Why it is attractive:** It is automatic and would find copies that people forget.
- **Why rejected:** I have not priced it. Tools of this kind generally need a person to set up and run them, and neither of the two hires is a compliance specialist. It is also aimed at a company with many systems, and Kloud Kart's data currently sits in one database and a set of files.

### Alternative B: Respond to a complaint when one arrives

- **How it would work:** Do nothing until a deletion request or complaint comes in, then search for the data.
- **Why it is attractive:** It costs nothing now.
- **Why rejected:** The 90-day deadline is a fixed constraint. With 300,000 people on the list, an unknown number of copies and no record of any, a search could not be done reliably in time. A missed copy also puts Kloud Kart in breach of what it told the customer.

## Consequences

- **What this buys:** Kloud Kart can show that it knows where customer data is kept and that each deletion was carried out. This supports its ability to answer a complaint within the 90 days. The software cost is close to zero.
- **What this costs:** Marketing and the Data Engineer must keep the register and log up to date, and the export rule slows each export by a few minutes. I have not measured how long that takes. It relies on people following the rule.
- **What this does not do:** A register and log do not by themselves make Kloud Kart compliant with the Act. The Act also requires registration with the Data Protection Commission, and the brief does not say whether Kloud Kart is registered. That is a separate task for the diagnosis questions and a lawyer.
- **What it costs the client if I am wrong:** If a copy is missing from the register, a customer's data could survive a deletion, and the company would have recorded a deletion that did not fully happen. To reduce this, the Data Engineer compares the register with the files actually held each quarter, and the export rule means a missing row is a breach of the rule, not an accident.
- **What would make me revisit this:**
  - The audit shows many more copies than ten, or dozens of systems hold customer data.
  - A copy is found that was not in the register.
  - Deletion requests rise well above the 5 a year assumed in A14.
  - The Nigeria pilot brings a regulator that requires automated proof of deletion.

## What this survives

- **Growth of 15–20% a year.** The register grows with the number of copies, not the number of customers, so a larger list does not make it harder to keep.
- **The Nigeria pilot.** I would add one column to the register recording which country's law applies to each copy. I have not checked Nigeria's data-protection rules, so a lawyer should confirm what they require before the pilot launches.
- **Only two hires.** It needs no new tool and no specialist, so it fits inside the existing plan.
- **A missed copy or a staff change.** The register and log live in a shared place, so they outlast any one person. The quarterly comparison catches copies that people forgot.

## Questions to settle before this is confirmed

- How many copies of the customer list exist, and where? (A16)
- What tools does Marketing use to send campaigns, and where do the lists come from?
- Is Kloud Kart registered with the Data Protection Commission, and has any deletion request already arrived?
- Who should own the register: the head of Marketing, or someone else?
