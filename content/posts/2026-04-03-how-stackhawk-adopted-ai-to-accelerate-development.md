---
date: '2026-04-03T10:00:00-06:00'
draft: false
title: 'How StackHawk Adopted AI to Drastically Accelerate Development'
summary: "Nine months of going all-in on AI-assisted development at StackHawk: the ramp-up, the custom tooling, and what actually made it stick."
tags:
    - ai
    - engineering-leadership
    - stackhawk
    - claude-code
canonicalURL: https://www.linkedin.com/pulse/how-stackhawk-adopted-ai-drastically-accelerate-dan-hopkins-sghac/
---

Is adopting AI-assisted development as hard as learning to code all over again? Or is it more like learning a new text editor—a few keybindings and you're off?

After nine months of going all-in on AI-assisted development at StackHawk, I'd say it's neither. It's more like learning a new programming language. Expect about three months of real ramp-up time. And unlike learning a new language, the syntax keeps changing underneath you.

Here's what we learned, what we built, and what actually made it stick.

## The Numbers Tell Part of the Story

Since May 2025, 60% of our commits have been AI-assisted. But that number undersells the transformation. Over six months, we saw an 8x increase in software output and deployments. In December alone, when I challenged the team with a pile of work to test our velocity, we launched 15 features.

But the numbers only tell part of the story. The real transformation is in how we work.

## What Made Adoption Stick

You can hand every developer an AI coding tool and watch adoption fizzle out within a month. We've seen it happen. What made it stick for us was a combination of factors that reinforced each other:

**Internal demos and presentations.** Engineers regularly show each other what's working. Someone figures out a better prompting pattern? They present it. Found a workflow that cut a two-day task to two hours? They demo it. These presentations did more than share knowledge—they helped crystallize understanding across the team. When developers started presenting, you could see them move from surface-level understanding to genuine mastery.

**Leadership modeling.** Managers and tech leads visibly use the tools. When leadership treats AI assistance as "how we work" rather than "optional new thing to try," adoption follows.

**Tracking the right metrics.** I used token consumption as an early proxy for adoption—about 75 million tokens per day tells me someone is using AI for multiple hours at a time. But usage metrics are just indicators. What we really cared about were outputs: deployments to production, pull requests created, and the velocity measured by our planning tools.

## The Three-Month Learning Curve

The ramp-up period followed a clear pattern. In the first month, developers were scratching the surface—basic prompts, simple use cases. By month three, they were getting deep into advanced features: planning mode, plugins, skills, subagents. The shift was visible in how they talked about the tools. Early conversations were tentative; later ones showed genuine fluency.

This progression matters because it's easy to confuse tool usage with productivity. Lines of code and token consumption are early indicators, but the real gains came when developers internalized new ways of working.

## We Built Our Own Infrastructure

Off-the-shelf tools got us started, but we quickly realized we needed to build infrastructure tailored to our codebase and workflows. The key insight: as much as possible, you want to give tools to AI.

In the before-AI times, debugging a production issue meant logging into an observability tool, then a log aggregator, then an error tracker—hopping from website to website, application to application. Now we build small wrappers that let Claude do what we used to do in the browser, with the added benefit of cross-correlating between different systems.

**Shared configuration.** Rather than each developer maintaining their own AI tool configs, we centralized configuration so improvements propagate to everyone. When someone figures out a better system prompt for our codebase, the whole team benefits immediately.

**Custom Claude Code skills.** We built plugins that understand StackHawk-specific patterns—our deployment process, our service architecture, our coding conventions. By wrapping tools in Claude Code skills, you don't even need to train the engineer beyond telling them the tool exists. The actual usage is left up to the AI, which can discover it or follow shared instructions.

**Metrics dashboard.** We built tooling to track AI usage across the team—commits, sessions, costs. This lets us understand adoption patterns and demonstrate ROI. (It's also how I can tell you that 60% number with confidence.)

**Error triangulation.** When debugging production issues, our developers can now query ALB logs, application logs, and Sentry in one command. The AI helps correlate across sources. What used to be 30 minutes of tab-switching is now a single query.

The other day, we got a report in a customer service channel about a bug in production. Using one of our AI tools, we debugged the problem in under five minutes and figured out exactly what was going on. This same investigation would have taken a senior engineer an hour or more—and now any engineer on the team can get that same experience.

**JIRA integration.** The official MCP integration wasn't working well for our workflows, so we built our own tooling for easier integration with our planning systems.

The ROI on these tools came fast—in some cases, within a couple of hours of building them. We kept everything simple: most tools are just wrappers around APIs, which minimizes maintenance. The shallow learning curve means adoption spreads quickly.

## AI Makes Better Security Tools

There's a particular irony in being an application security company that's gone all-in on AI-assisted development. We build tools that help developers find and fix security vulnerabilities. Now AI helps us build those tools faster.

The result: features that would have taken a sprint now ship in days. Customer-requested improvements that sat in the backlog for months are getting addressed.

Our customers benefit directly. When we can iterate faster on our security scanning engine, we can detect more vulnerabilities, reduce false positives faster, and respond to new threat patterns more quickly. The speed compounds.

## The Productivity Story

One concrete example: we transformed our internal customer support channel. When engineering questions come in, we now have AI assistance that draws from our codebase and builds its own knowledge base from previous Q&A interactions. The engineering team spends less time answering the same questions repeatedly, and the answers get better over time.

With AI handling initial diagnostics and debugging, engineers can focus on more complex tasks. It's not just about doing the same work faster—it's about optimizing how we allocate engineering talent.

## What I'd Tell Another VP of Engineering

**Budget for the ramp-up.** Three months is realistic. Don't expect immediate productivity gains—there's a learning curve, and your developers need time to develop new intuitions. Watch for the shift from surface-level usage to genuine fluency.

**The tools will change.** What we use today won't be what we use in a year. Build your adoption strategy around learning how to learn rather than mastering a specific tool.

**Make it visible.** Internal demos, shared configs, metrics dashboards—anything that makes AI-assisted work visible helps adoption spread.

**Build for your context.** Generic tools get you started. Custom tooling for your specific codebase and workflows is where the real gains come from. Keep the tools simple—wrappers around APIs—and the ROI comes fast.

**Accept some quality trade-offs.** Higher velocity may mean slightly more bugs. Build your automated checks accordingly, and maintain human oversight where it matters most.

We're still early in this transformation. The 60% of AI-assisted commits will probably look quaint in a few years. But the infrastructure we've built and the culture we've developed will compound. That's the real investment.

---

*Originally published on [LinkedIn](https://www.linkedin.com/pulse/how-stackhawk-adopted-ai-drastically-accelerate-dan-hopkins-sghac/).*
