---
date: '2026-06-18T10:00:00-06:00'
draft: false
title: 'How to Stop AI From Lying to You With "Balance"'
summary: "Everyone worries about AI making things up or refusing to answer. The trickier failure is the calm, balanced-sounding answer that treats settled science and bad-faith contrarianism as a coin flip. Here's how to spot it and ask your way past it."
tags:
    - ai
    - epistemics
    - critical-thinking
cover:
    image: /images/balance-rigged-scale-cover.png
    alt: "A balance scale sitting perfectly level: a tall stack of peer-reviewed books on one pan, a single small megaphone on the other — the visual of false balance."
    relative: false
---

## The thing AI is genuinely great at

One of the best uses I've found for AI is having the conversation I've always wanted to have but never had the right person for. How does relativity actually work? How does the Fed moving interest rates ripple out into the rest of the economy? With a little distance now, what does the evidence really say about how we handled COVID?

I'd carried questions like that around for years. The answers exist — I just didn't know anyone with real depth in the right area. My doctor knows medicine, not monetary policy. The friend who can walk me through relativity checks out the moment immunology comes up. With AI you can finally chase a question wherever it goes, with something that knows a little about almost everything, and you can ask the dumb follow-up you'd be too embarrassed to put to a real expert.

## The third failure mode

Ask an AI about something contested — COVID vaccines, puberty blockers, where the virus came from — and you're probably braced for one of two ways it can go wrong. Either it makes something up, or it clams up and won't engage.

There's a third way, and it's the one that actually gets people. The answer comes back perfectly even-handed, and that even-handedness quietly misrepresents what's known, because it treats settled expert opinion and politically motivated contrarianism as if they carry the same weight. It reads as careful, and because it reads as careful, it's the version most likely to fool you. It's epistemological theater: the look of rigor without any of the work.

Here's the shape of it. Ask how well the COVID vaccines worked and you'll get something like, "Supporters point to reduced mortality, while critics raise concerns about transmission and shifting guidance." Sounds reasonable. But it smuggles the political fight over how the vaccines were talked about into what's dressed up as a scientific fight over whether they worked. Those are two different arguments.

The "both sides" instinct mashes together three things that aren't the same:

- Real scientific uncertainty, where the evidence genuinely has gaps and experts are still arguing.
- Communication failure, where the science was fine but the people explaining it did a bad job.
- Manufactured controversy, where someone with an agenda is using the topic as a wedge.

All three are real, and each one calls for a different response. Blur them together and you get an answer that feels balanced while pointing you the wrong way.

## The moves to watch for

Once you start looking, the same handful of moves show up again and again.

The first is padding the case with things that don't belong to it. You ask a narrow question, and the answer drags in a pile of adjacent examples to make a thin point look heavy. Ask about federal vaccine mandates and you'll hear about military shots, school requirements, employer rules — all of which have been around forever — as if they add up to some sweeping new overreach. Press on it and the whole thing shrinks to a single OSHA rule that got struck down before it ever took effect.

The second is measuring against the wrong yardstick. The COVID vaccines get held up against something like the measles vaccine, which gives you sterilizing, lifelong immunity, instead of the flu vaccine, which is the honest comparison for a fast-mutating respiratory virus. Judge them against the flu shot and the "failure" mostly evaporates.

The third is treating four different kinds of failure as one big one. The science being wrong, the messaging being bad, the institutions losing trust, the policy going too far — those are separate problems. Lump them together and a botched press conference starts to count as proof the science was wrong, or an overreaching mandate becomes evidence the consensus was cooked up.

The last is handing too much credibility to whoever happens to be standing nearby politically. An epidemiologist with thirty years of peer-reviewed work flagging a specific methodological problem is not the same as a senator quoting that flag in a press release. AI tends to blur the two instead of saying out loud which is which.

## How to prompt your way out

You're not trying to get the AI to pick a side. You're trying to get it to lay out what's actually known — where the evidence is solid, where it's thin, and where politics has crept into a question that should be empirical. A few prompts pull it back in that direction.

Ask what the experts actually think, and ask it first: "What do people with peer-reviewed work in this field actually conclude?" You want the consensus on the table before any of the noise gets stirred in.

Ask who's who: "Which of these sources are domain experts, and which are commentators or politicians?" A clinical trial, a systematic review, an observational study, a policy memo, and a press release are not the same animal, and it's worth making the AI say so.

Ask what kind of disagreement this is: "Are scientists actually split here, or has a settled question just gotten politically hot?" That one question separates "we don't know" from "people are mad," which look identical from the outside and need completely different answers.

Ask about the yardstick: "What's the right thing to be comparing this against?" It catches the wrong-comparison move before it warps everything downstream.

And make it argue both ends honestly: "Give me the strongest version of the mainstream position, then the strongest fair objection to it." You get the real expert view that way, instead of one that's been watered down in advance.

## The bigger picture

There's a reason this keeps happening. We're living through a stretch where expertise has been knocked down on purpose, as a political strategy. People who actually spent years learning how something works — putting in the time, getting things wrong, answering to peers who'd catch them — keep getting set on the same level as people whose real talent is sounding confident.

And it picks this up in a specific, mechanical way. The frontier labs — Anthropic, which makes Claude, among them — train their models with reinforcement learning from human feedback: people are shown competing answers, pick the one they like better, and those preferences get distilled into a reward the model learns to chase. It's a genuinely powerful technique, and it's also exactly where the bias creeps in. When the raters have spent a decade marinating in "both sides" coverage, they reward the answer that feels balanced even when the balance is fake, and the model dutifully learns that sounding even-handed is what earns the high score. Train a system to chase human approval and it will reproduce human blind spots faithfully — our reflex to treat every disagreement as two reasonable sides included. So you end up with a system that can mislead you in the calmest, most reasonable-sounding voice imaginable.

The fix isn't an AI that takes sides. It's one that's epistemically honest — one that'll come right out and say when a "debate" isn't being argued in good faith, when the science is solid even though the topic is radioactive, and when the controversy is mostly manufactured. Getting that out of it still falls partly on you, and on knowing what to ask.

## Where to build these skills

A couple of things sharpened how I think about this:

- **[Professor Dave Explains](https://www.youtube.com/@ProfessorDaveExplains)** has spent years taking apart pseudoscience on YouTube. The debunks themselves are fun, but the real payoff is watching him name the moves — the cherry-picked study, the lone credentialed crank, the scrap of genuine uncertainty stretched into a wall of doubt. Once you've seen them named, you can't unsee them, including in what an AI hands you.
- **[Decoding the Gurus](https://decoding-the-gurus.captivate.fm/)** has an anthropologist and a psychologist pick apart the "secular gurus" — the people who build big followings playing the brave outsider. Their method for telling a real independent thinker from someone just performing the role is exactly what you want in your back pocket when an AI starts "presenting multiple perspectives."

---

*This came out of two long back-and-forths with Claude, one about COVID vaccine science and one about pediatric gender medicine. Same pattern both times: the moment the topic got politically charged, it slid into false balance and treated peer-reviewed consensus and motivated contrarianism as roughly a coin flip. It took real pushing to get to a straight answer. Annoying conversations — but a good map of where the failure actually lives.*
