---
date: '2026-04-24T10:00:00-06:00'
draft: false
title: 'How I Built an Agentic Family Office'
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

## The data room

When a company raises money or gets acquired, the lawyers set up a *data room*: one organized folder with every contract, every cap table, every insurance binder, every lease, so a stranger can understand the whole entity in an afternoon. I built the same thing for my household.

The repo *is* the data room. It's just folders of Markdown and PDFs, but the shape is the point:

```text
finances/
├── CLAUDE.md              # operating manual — who this agent is, what it owns, the house rules
├── .claude/
│   ├── agents/            # the specialist subagents (CIO, Risk, Tax, Estate, Contrarian)
│   ├── commands/          # slash commands for repetitive jobs (e.g. match-venmo)
│   └── skills/            # reusable procedures (check bank sync, reconcile schedules)
├── docs/                  # the memos — structured knowledge, hand-written, kept current
│   ├── accounts.md        # every account: balance, titling, sync status
│   ├── insurance.md       # every policy: limits, definitions, premiums, beneficiaries
│   ├── investments.md     # allocations, fees, advisor, cost basis
│   ├── loans.md           # mortgage, HELOC, solar, lease — rates and amortization
│   ├── planning.md        # retirement targets, risk tolerance, emergency fund, the plan
│   ├── people.md          # the household facts an advisor asks on day one
│   └── ...                # budget, categories, rates, processes, a running changelog
├── policies/              # the source documents — the actual PDFs behind every memo
├── data/                  # raw exports — utility usage, 15-minute meter data
├── scripts/               # parsers that turn statement PDFs into ledger rows
└── taxes/ → (symlink)     # years of returns, W-2s, 1099s — lives outside the repo
```

Two layers do the work. `docs/` holds the **memos**: hand-written Markdown that distills each domain down to the facts and numbers that matter. `policies/` holds the **primary sources**: the actual PDFs, so when an agent needs the exact own-occ definition or the precise deductible, it reads the binder, not my summary of it.

`CLAUDE.md` at the root is the operating manual. It tells any agent who it is, which accounts exist, what tools to reach for, and the house rules (verify before claiming fixed; never paper over drift with a fake reconciliation entry). It's the thing that turns a generic LLM into *my* bean counter instead of a chatbot guessing.

And the whole thing is version-controlled. Every change to the plan, every reconciliation, every "we finally raised the auto liability" is a commit. The data room has a history I can walk backward through.

## What I feed it

Most people assume their finances are too boring, too private, or too messy to hand to an AI. Mine knows essentially everything — and every category of data unlocked something. Here's the inventory, anonymized. Use it as a checklist for your own:

- **Every account.** Checking, savings, brokerage, credit cards — pulled automatically into one ledger, with full transaction history going back years.
- **Every insurance policy.** Home, auto, health, dental, vision, disability, life, pet, and a spouse's business liability. Not the summaries — the actual binders, with limits, definitions, and beneficiaries.
- **Every investment statement.** IRAs, Roth, the kid's 529, the 401k, the HSA — allocations, expense ratios, advisory fees, cost basis lot by lot.
- **Every paycheck.** Not just the net — the full deduction stack: 401k, HSA, insurance premiums, the employer-paid benefits I never see as income. Comp history across every job. The equity grant, strike price, and post-termination exercise windows.
- **Every debt.** Rates, terms, payoff math, full amortization tables.
- **Utility data.** Electric bills going back two years, plus the 15-minute interval meter export. (Yes, the family office knows when the dryer runs.)
- **Years of tax returns.** W-2s, 1099s, a Schedule C for the side business, filed returns back two decades.
- **The household itself.** Names, dates of birth, who's the beneficiary on what, the things an advisor asks on day one.
- **The plan.** Retirement target, risk tolerance, emergency-fund strategy, the kid's education plan, the big expenses coming — written down, not in my head.

The unlock isn't any single document. It's having *all* of it in one place, in a form an agent can read, cross-reference, and reason over at once. The disability buy-up question only had an answer because the policy PDF, the paycheck deductions, and the planning doc were sitting in the same room.

On privacy, since it's the first thing everyone asks: this runs on my own hardware. The data room is a folder on a server in my house.

## The roles

I wrote a handful of specialist "agents." Each one is a prompt with a specific mandate, a reading list of documents about our finances, and a fixed output format. The framing across all of them is the same: a recommendation has been proposed, and the agent's job is to pressure-test it from one specific lens, then return a verdict, pushback, and what's missing.

- **Chief Investment Officer.** Long-horizon and allergic to cost. Reviews anything that touches investments, savings priorities, cash allocation, or retirement funding. Looks for fee drag (basis points compounded over decades), concentration risk, allocation drift from the stated risk tolerance, opportunity cost versus tax-advantaged alternatives, and decisions that quietly hurt withdrawal flexibility in retirement. The question it keeps asking: "is there a cheaper way to get the same exposure?"
- **Risk / Insurance Officer.** Thinks in probability × severity, not premiums. Reviews insurance and liability decisions for coverage adequacy against income and net worth, coordination between policies (the offsets and underlying-coverage requirements that surface only at claim time), definition quality (own-occ vs. any-occ, replacement cost vs. actual cash value), and portability when the job goes away. Distinguishes risks that would end the retirement plan from risks that would just sting, and only aggressively hedges the former.
- **Tax Strategist.** Multi-year, not single-year. Hunts for tax alpha left on the table — under-funded tax-advantaged vehicles, missed Roth-vs-Traditional decisions at the current marginal bracket, bunching opportunities for charitable giving, withholding that's producing a fat refund instead of working all year. Equally watchful for future tax cliffs the recommendation creates: RMDs pushing Social Security taxability, IRMAA Medicare surcharges, AMT exposure on option exercises. Knows the difference between marginal and effective rates and which one belongs in which calculation. Tells me when a question needs the actual CPA, not an agent.
- **Estate Planner.** "If you both died tomorrow, does the asset path work?" Reviews beneficiary designations, account titling (which overrides wills for many assets), guardianship for minor children, powers of attorney, healthcare directives, and digital-asset access. Distinguishes incapacity from death, since the former is often longer and more legally tangled. Flags the difference between something a beneficiary form can fix in an afternoon and something that needs a real attorney to paper.
- **Contrarian.** Exists because every financial decision has a plausible case for the opposite choice, and homes without a contrarian voice systematically underweight downside scenarios. Steelmans the opposite of whatever the others just agreed on. Names the load-bearing assumption everyone is sharing without saying it out loud. Sketches the specific future in which the decision looks dumb in ten years. Loudest when all the other specialists agree, because that is when groupthink is most expensive.

Any meaningful decision (large, irreversible, or multi-year) gets routed through the relevant subset. The Contrarian always gets a vote. They disagree with each other constantly, which is the whole point. It stops me from getting one confident answer that happens to be wrong.

## Anatomy of an agent

There's no framework here and no orchestration code. Each specialist is one Markdown file with a strict shape. Here's the skeleton every one of them shares:

```yaml
---
name: family-office-contrarian
description: When to invoke this agent — the router reads this line to decide.
tools: Read, Grep, Glob, Bash      # read-only: agents critique, they don't touch the ledger
model: sonnet
---
```

Then four sections in the body: a **mandate** (the one job), a **context-to-load** list (which docs to read before opening its mouth), a **review lens** (the specific questions it's obligated to ask), and a fixed **output format** so every agent returns the same shape — Verdict, Pushback, What's Missing. That uniform output is what lets me line up five opinions side by side and see where they collide.

Here's the Contrarian in full — the most reusable of the five, and the one with the least of me baked into it. Drop it in your own `.claude/agents/` and it works as-is:

```markdown
---
name: family-office-contrarian
description: Devil's-advocate perspective for the family office. Stress-tests
  recommendations by finding the case where they're wrong, questioning the
  assumptions everyone shares, and surfacing blind spots the other specialists
  miss. Use on any significant recommendation before acting — especially when
  all the other agents agree (that's where groupthink is most expensive).
tools: Read, Grep, Glob, Bash
model: sonnet
---

You are the designated contrarian for the family office. Your job is to find
the case where the recommendation is wrong, to question the assumptions
everyone else took for granted, and to surface the blind spots the rest of the
team shares.

You exist because **every financial decision has a plausible case for the
opposite choice**, and households without a contrarian voice systematically
under-weight downside scenarios.

## Your mandate

The recommender has proposed a decision, and typically the other specialists
(CIO, Risk, Tax, Estate) have weighed in. Your job is to:

1. **Find the case for NOT doing this** — even if it feels obvious
2. **Question load-bearing assumptions** — the thing everyone thinks is true
   that might not be
3. **Surface shared blind spots** — what the other agents all missed because
   they share the same framework
4. **Explore the regret case** — the future where you look back in 10 years and
   wish you'd made the opposite choice

## Context to load before responding

- `docs/planning.md` — but read it skeptically
- `docs/people.md` — especially income concentration and career risk
- Whatever specific doc the recommendation touches (insurance, accounts, etc.)

## Review lens

1. **"Why would a smart person disagree?"** — Articulate the steelmanned
   opposite case. What's the best argument for not doing this?
2. **"What's the unexamined assumption?"** — Most recommendations rest on
   unstated premises. Name them.
3. **"What future would make this look dumb?"** — Job loss, a parent needing
   long-term care, a child needing special-needs support, divorce, major
   illness, the concentrated asset going to zero. Which of these break it?
4. **"What would an index-fund purist / FIRE maximalist / debt-free absolutist
   say?"** — Adopt a different worldview and see what breaks.
5. **"Is this optimizing for the wrong thing?"** — Solving for psychological
   comfort when the answer is math, or vice versa?
6. **"What's the opportunity cost of the bandwidth?"** — Is this decision even
   important enough to warrant the analysis?
7. **"What would a fee-only fiduciary with nothing to sell say?"** — Is there a
   product or commission bias hiding in the recommendation?
8. **"Small-number-large-number problem?"** — Does this look small today but
   compound to huge tomorrow (or vice versa)?

## Output format

Respond in under 400 words:

**THE CASE FOR NOT DOING THIS**: The strongest opposing case in 2-3 sentences.
No hedging.

**UNEXAMINED ASSUMPTIONS**: 2-3 load-bearing things the recommender is assuming
without naming.

**THE REGRET SCENARIO**: The specific future where you wish you'd done the
opposite. What breaks?

**A DIFFERENT FRAMING**: Re-cast the decision from a perspective the recommender
didn't use.

**BOTTOM LINE**: Either (a) the pushback dissolves — the recommendation survives
the stress test — or (b) reconsider, given [specific reason].

## Principles

- Be useful, not obstructionist. The goal is better decisions, not paralysis.
- Steelman, don't strawman. The opposite case has to be one a smart person could
  genuinely hold.
- Be specific. "There's always risk" is useless. Name the number, the year, the
  mechanism.
- If after pressure-testing the recommendation still holds, say so clearly.
- Notice when all the other specialists share a framework (they all assume the
  job is stable, they all assume the same allocation style) and probe whether
  that framework is load-bearing for the recommendation.
```

The other four are the same file with a different lens. The CIO loads the investment docs and keeps asking "is there a cheaper way to get the same exposure?" The Risk Officer thinks in probability × severity and reads the actual policy binders. The Tax Strategist works in multi-year horizons. The Estate Planner asks "if you both died tomorrow, does the asset path work?" Same skeleton, five different obsessions.

What makes them useful isn't the model — it's the constraints. A reading list keeps them grounded in my real numbers instead of generic finance-blog advice. A word limit forces a verdict instead of a hedge. Read-only tools mean they can argue but never quietly "fix" the ledger. And a mandate to *push back* means they have to earn their place; an agent that always agrees is just latency.

## Where it paid off

I fed every policy we have into the repo: home, auto, health, disability (personal and work), short- and long-term, dental, vision, life (term, whole life, two VULs), horse, dogs, Steph's business liability.

The findings were not subtle:

- Our auto liability was at **Colorado state minimum** despite a net worth long past the point where that made sense. One bad intersection and we would have lost almost everything. Fixed.

A human family office would have found all of this too. The difference is that this one is always running, never bills an hour, and can look at something before breakfast because I had a weird thought in the shower.

## Firing the advisor

The hardest call the family office pushed me toward: we ended a years-long relationship with our financial advisor and moved the money into a self-managed portfolio of low-cost index funds.

This is exactly what the CIO agent exists to catch. Its obsession is fee drag — basis points compounded over decades — and it kept landing on the same math: we were paying an advisory fee *on top of* the expense ratios of the funds inside the portfolio, for active management that wasn't beating the index it was being measured against. Two layers of cost, every year, on a balance meant to compound for another thirty.

I made the Contrarian argue the other side before I touched anything. It gave the honest case for staying: an advisor's real value isn't picking funds, it's behavioral — being the voice that stops you from selling at the bottom. The fee is partly insurance against your own worst instinct in a crash. That's a real argument. But I decided I could write that discipline into the plan and the cron job, and keep the basis points.

So we left. The exit cost an afternoon of paperwork and a slightly awkward phone call. The savings recur every year, for the rest of the portfolio's life.

## The point

This does not replace our CPA or our estate attorney. I still use both. What it replaces is me showing up under-prepared, asking vague questions, accepting the first answer, forgetting to follow up. Now I walk in with the right questions and the documents already pulled. At least that's how it works today...

The tooling is unglamorous. A cron job and a folder of Markdown. The real upgrade is the posture it creates, treating our own finances with the seriousness a founder gives their company.
