---
date: '2026-04-24T10:00:00-06:00'
draft: false
title: 'How I Built a Family Office'
summary: "A DIY version of what the rich have: a team of specialists coordinating your money, running on my home server. What is in it, what it caught, and why the posture matters more than the tooling."
tags:
    - ai
    - homelab
    - finance
    - family-office
---

Most families have a budget spreadsheet. A few have a financial advisor. Very few have a family office, the team of specialists that rich families hire to look at their money from every angle at once. A CIO for investments. A tax strategist. An estate attorney. A risk officer for insurance. Someone whose whole job is to say "have you considered the opposite?"

I am not rich enough for any of that. But I built one anyway. It runs on my home server, costs almost nothing, and has already paid for itself.

## The plumbing

Three layers, all open and boring:

- **[SimpleFIN](https://www.simplefin.org/)** pulls transactions from every bank, credit card, and brokerage into one place.
- **[Actual Budget](https://actualbudget.org/)** (self-hosted) is the ledger and the envelope budget.
- **[Claude Code](https://www.anthropic.com/claude-code)** is the brain. A cron job runs it every morning. It categorizes new transactions, matches Venmo charges against email receipts, flags anything weird, and emails me a short report. On Sundays it writes a weekly review. On the first of the month it reconciles.

That layer alone is worth the afternoon it took to set up. But the interesting part is above it.

## The roles

I wrote a handful of specialist "agents." Each one is a prompt with a specific mandate, a reading list of documents about our finances, and a fixed output format. The framing across all of them is the same: a recommendation has been proposed, and the agent's job is to pressure-test it from one specific lens, then return a verdict, pushback, and what's missing.

- **Chief Investment Officer.** Long-horizon and allergic to cost. Reviews anything that touches investments, savings priorities, cash allocation, or retirement funding. Looks for fee drag (basis points compounded over decades), concentration risk, allocation drift from the stated risk tolerance, opportunity cost versus tax-advantaged alternatives, and decisions that quietly hurt withdrawal flexibility in retirement. The question it keeps asking: "is there a cheaper way to get the same exposure?"
- **Risk / Insurance Officer.** Thinks in probability × severity, not premiums. Reviews insurance and liability decisions for coverage adequacy against income and net worth, coordination between policies (the offsets and underlying-coverage requirements that surface only at claim time), definition quality (own-occ vs. any-occ, replacement cost vs. actual cash value), and portability when the job goes away. Distinguishes risks that would end the retirement plan from risks that would just sting, and only aggressively hedges the former.
- **Tax Strategist.** Multi-year, not single-year. Hunts for tax alpha left on the table — under-funded tax-advantaged vehicles, missed Roth-vs-Traditional decisions at the current marginal bracket, bunching opportunities for charitable giving, withholding that's producing a fat refund instead of working all year. Equally watchful for future tax cliffs the recommendation creates: RMDs pushing Social Security taxability, IRMAA Medicare surcharges, AMT exposure on option exercises. Knows the difference between marginal and effective rates and which one belongs in which calculation. Tells me when a question needs the actual CPA, not an agent.
- **Estate Planner.** "If you both died tomorrow, does the asset path work?" Reviews beneficiary designations, account titling (which overrides wills for many assets), guardianship for minor children, powers of attorney, healthcare directives, and digital-asset access. Distinguishes incapacity from death, since the former is often longer and more legally tangled. Flags the difference between something a beneficiary form can fix in an afternoon and something that needs a real attorney to paper.
- **Contrarian.** Exists because every financial decision has a plausible case for the opposite choice, and homes without a contrarian voice systematically underweight downside scenarios. Steelmans the opposite of whatever the others just agreed on. Names the load-bearing assumption everyone is sharing without saying it out loud. Sketches the specific future in which the decision looks dumb in ten years. Loudest when all the other specialists agree, because that is when groupthink is most expensive.

Any meaningful decision (large, irreversible, or multi-year) gets routed through the relevant subset. The Contrarian always gets a vote. They disagree with each other constantly, which is the whole point. It stops me from getting one confident answer that happens to be wrong.

## Where it paid off

I fed every policy we have into the repo: home, auto, health, disability (personal and work), short- and long-term, dental, vision, life (term, whole life, two VULs), horse, dogs, Steph's business liability.

The findings were not subtle:

- Our auto liability was at **Colorado state minimum** despite a net worth long past the point where that made sense. One bad intersection and we would have lost almost everything. Fixed.
- The disability stack coordinates **cleanly**. The employer policy does not offset against our personal ones, which changed the math on a buy-up offer about to expire.
- A pending disability buy-up option had a **definition-inheritance** question I would have missed. The specialists caught it on the first pass.

A human family office would have found all of this too. The difference is that this one is always running, never bills an hour, and can look at something before breakfast because I had a weird thought in the shower.

## The point

This does not replace our CPA, estate attorney, or financial advisor. I still use all three. What it replaces is me showing up under-prepared, asking vague questions, accepting the first answer, forgetting to follow up. Now I walk in with the right questions and the documents already pulled. At least that's how it works today...

The tooling is unglamorous. A cron job and a folder of Markdown. The real upgrade is the posture it creates, treating our own finances with the seriousness a founder gives their company.
