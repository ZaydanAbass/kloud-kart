# Improvement beyond the brief: A simple web app to inform Kloudkart on stock

## The problem it solves
The brief states that stock counts cannot go stale, or Kloud Kart will oversell. However, Nana currently has no way of knowing if the numbers she is looking at are fresh. She only finds out about an oversell when a customer complains. 

## What I built
A simple web page for Nana with two parts:
1. A freshness badge: It tells her exactly how old the stock numbers are (e.g., "last updated 40 seconds ago"). It turns green when the numbers are fresh, amber as they get older, and red when they are too old to trust.
2. An oversell finder: A list of items where more has been sold than was actually in stock, so Nana can contact affected customers before they complain.

For the walkthrough, this uses sample data and a slider to simulate the delay. In real life, it simply reads the pre-approved database copy without touching the live website.

## What it is worth to Nana and Kloudkart
She knows immediately if she can trust her Monday reordering numbers.Kloudkart can also fix oversells proactively rather than reactively. Even at a conservative estimate of just two oversold orders a month, fixing them costs roughly GH₵1,200 a year in refunds and staff time. Stock Watch gives her a visual safety net to prevent this.

## What it cost
Zero cedis in software. It uses tools the team already has, requires no new servers, and does not touch the live website.