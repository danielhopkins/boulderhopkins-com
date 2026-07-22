---
date: '2026-07-21T09:00:00-06:00'
draft: false
title: 'Meld: A Score Keeper for Game Night'
thumbnail: /images/meld-icon.png
tags:
    - ios
    - swift
    - swiftui
    - design
    - cards
---

Every game night in this house ends the same way: somebody grabs the nearest envelope, draws a wobbly grid on the back of it, and we keep score there until the columns stop lining up. Then somebody asks who won last time and nobody knows.

So I built **Meld**, an iOS score keeper for card and board games. It's in TestFlight now, and the first beta [just cleared Apple's review](https://testflight.apple.com/join/FxQT6A5C).

{{< rawhtml >}}
<style>
  .shots {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: flex-start;
    gap: 1.75rem;
    margin: 2.25rem 0 0.9rem;
  }

  /* Device bezel — brushed titanium edge, deep inner rail, soft table shadow. */
  .shot {
    margin: 0;
    width: 258px;
    max-width: 100%;
    padding: 7px;
    border-radius: 38px;
    background:
      linear-gradient(150deg, #6e6e75 0%, #2b2b30 18%, #111114 48%, #2b2b30 82%, #77777e 100%);
    box-shadow:
      0 22px 45px rgba(0, 0, 0, 0.26),
      0 6px 14px rgba(0, 0, 0, 0.16),
      inset 0 0 0 1px rgba(255, 255, 255, 0.16);
    transition: transform 0.35s ease, box-shadow 0.35s ease;
  }

  .shot img {
    display: block;
    width: 100%;
    height: auto;
    border-radius: 31px;
    box-shadow: inset 0 0 0 1px rgba(0, 0, 0, 0.6);
  }

  .shot:hover {
    transform: translateY(-5px);
    box-shadow:
      0 32px 60px rgba(0, 0, 0, 0.32),
      0 8px 18px rgba(0, 0, 0, 0.2),
      inset 0 0 0 1px rgba(255, 255, 255, 0.22);
  }

  [data-theme="dark"] .shot {
    box-shadow:
      0 22px 45px rgba(0, 0, 0, 0.55),
      0 6px 14px rgba(0, 0, 0, 0.35),
      inset 0 0 0 1px rgba(255, 255, 255, 0.2);
  }

  [data-theme="dark"] .shot:hover {
    box-shadow:
      0 32px 60px rgba(0, 0, 0, 0.62),
      0 8px 18px rgba(0, 0, 0, 0.4),
      inset 0 0 0 1px rgba(255, 255, 255, 0.26);
  }

  /* Hero: one phone, slightly larger. */
  .shots--solo .shot { width: 300px; padding: 8px; border-radius: 44px; }
  .shots--solo .shot img { border-radius: 36px; }

  .shots-caption {
    text-align: center;
    font-size: 0.85rem;
    color: var(--secondary);
    margin: 0 0 2.25rem;
  }

  @media (max-width: 620px) {
    .shots { gap: 1rem; }
    .shot { width: 45%; padding: 4px; border-radius: 24px; }
    .shot img { border-radius: 20px; }
    .shots--solo .shot { width: 72%; padding: 6px; border-radius: 34px; }
    .shots--solo .shot img { border-radius: 28px; }
  }

  @media (prefers-reduced-motion: reduce) {
    .shot, .shot:hover { transition: none; transform: none; }
  }
</style>

<div class="shots shots--solo">
  <figure class="shot">
    <img src="/images/meld-board.png" alt="The Meld scorepad for a game of Spades, showing two teams, four rounds, bids, and a running total">
  </figure>
</div>
<p class="shots-caption">The scorepad: two teams, four rounds, bids turned on partway through.</p>
{{< /rawhtml >}}

## What it does

Set up a game and it asks only the questions that actually vary: individuals or teams? High score wins, or low? Is there a target to play to?

From there you're on the **scorepad** — a running grid, one column per player or team, one row per round. Tap a cell to enter a score, add a note, or keep a tally when you're counting tricks rather than points. Close the round and it captures every active column at once. Games that end get archived with a winner, so the question of who won last time has an answer.

The part I care most about is that it stretches. Trash barely needs more than a number a round. Wizard and Oh Hell want a bid recorded next to what you actually took. Euchre wants to know who called trump. Spades wants bags, which sit there accumulating until ten of them cost you a hundred points. Those are wildly different scorepads on paper, and in Meld they're the same grid with different columns switched on — you enable what the game needs and never see the rest.

A few more things the paper envelope never gave me:

- **The game can change while you're playing it.** Somebody shows up at round four. Somebody leaves. You realize halfway in that you should have been tracking bids all along. Columns and seats get added mid-game, and the rounds before them keep an honest blank instead of a wrong zero — the scorepad above says *"Bid turned on at round 3"* for exactly that reason. Under the hood it's a sparse column-value model rather than a fixed table, which is what makes that a normal Tuesday instead of a reason to start the game over.
- **Notes that stay put.** House rules pin to the top of the scoreboard, so the "ten bags is minus a hundred, we agreed" argument gets settled by looking up instead of relitigating. Individual cells take notes too — that *renege penalty* on round four is somebody's evening being remembered forever. This matters most for the games that never really end: the forever gin rummy match, where the running total is honestly less interesting than the accumulated grievances.
- **Presets.** The people you play with most, ready to select, so setup is a few taps rather than typing four names again.
- **Score corrections.** Because somebody always mis-adds, and fixing it shouldn't mean rebuilding the grid.

{{< rawhtml >}}
<div class="shots">
  <figure class="shot">
    <img src="/images/meld-setup.png" alt="Meld's new game setup screen with name, players, format, target, and note fields">
  </figure>
  <figure class="shot">
    <img src="/images/meld-finished.png" alt="Meld's winner screen showing Dan &amp; Mia at 160 out of 300 with final standings">
  </figure>
</div>
<p class="shots-caption">Setup asks only what varies between games. Finishing one files it away with a winner.</p>
{{< /rawhtml >}}

## The reference

The feature I didn't plan on is the one I use most. There's a "?" in the corner that opens a reference of **20 classic games** — Spades, Hearts, Euchre, Oh Hell, Pinochle, Canasta, Cribbage, Nerts, Kings in the Corner, and the rest — with how to deal, how to play, and how scoring actually works.

It exists because the real failure mode at game night isn't the arithmetic. It's four adults arguing about whether you can go out on a discard in Rummy 500. Search understands alternate names too, since half these games are known by two or three of them depending on who taught you.

{{< rawhtml >}}
<div class="shots">
  <figure class="shot">
    <img src="/images/meld-reference.png" alt="Meld's card game reference listing Spades, Hearts, Euchre, Five Hundred, Oh Hell, Wizard, and more">
  </figure>
  <figure class="shot">
    <img src="/images/meld-spades.png" alt="The reference entry for Spades, showing deck, deal, how to play, and scoring rules">
  </figure>
</div>
<p class="shots-caption">Twenty games, each with the deal, the play, and the scoring that people actually argue about.</p>
{{< /rawhtml >}}

## Why "Meld"

The honest answer is that the score was never the point. Card games are one of the few things that reliably get everyone around the same table for three hours, arguing cheerfully about whether that was a renege. The scorepad is the excuse — the small shared object in the middle that keeps the evening pointed at itself.

Which makes this a slightly funny thing to have built, since my version of that shared object is a phone, at a table full of people who came to look at each other instead of at screens. So the bar it has to clear is earning the spot: quick to put a number in, then back down on the table. Every design argument I had with myself came down to that. I wanted a name that carried it, too, rather than describing a utility.

"Meld" does both jobs in one word. It's a genuine card term — you meld runs and sets in rummy, canasta, and pinochle — so it belongs specifically to this table rather than to score-keeping in general. And outside of cards it just means to fuse, to join separate things into one. That's the actual product: cards on the surface, people around the table underneath.

Two names I liked lost to it. **Showdown** is evocative and hopelessly crowded — the bare title is taken, plus Draft Showdown, Bingo Showdown, and a half-dozen sports variants; for an app with no ad budget and no plan beyond organic search, that collision is disqualifying. **Scorepad** I liked enough to keep as the in-app word for the grid, but it's effectively a category noun with a direct competitor already sitting on it. Neither of them meant anything beyond the mechanics, which in hindsight is why they were easy to give up.

## The beta

It's iOS only, it needs iOS 26, and it is genuinely a beta — the whole thing is about two weeks old. If you play cards and want to try it:

**[Join the Meld beta on TestFlight →](https://testflight.apple.com/join/FxQT6A5C)**

What I'd most like to know: does it survive a real game, start to finish, with real people who don't know how it's supposed to work? That's the test the simulator can't run.

As with [MiniMusic](/posts/2026-07-16-minimusic-learns-to-listen/), I built this with [Claude Code](https://claude.ai/code) doing the heavy lifting — including, at one point, an agent whose entire job was researching a card game and writing it into the reference. Twenty games got added faster than I could have read the rules for five.
