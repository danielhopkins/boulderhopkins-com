---
date: '2026-07-16T10:00:00-06:00'
draft: false
title: 'MiniMusic Learns to Listen: On-Device AI Search'
thumbnail: /images/minimusic-icon.png
tags:
    - macos
    - swift
    - swiftui
    - music
    - ai
---

[MiniMusic](https://github.com/danielhopkins/MiniMusic) is my little menu bar app for controlling Apple Music from the keyboard: global hotkey, type, return, music. The new release, [MiniMusic 26.716.1](https://github.com/danielhopkins/MiniMusic/releases/tag/v26.716.1), is the biggest one yet, because the search box got a brain.

## Search that understands what you meant

Until now, search did what search boxes do: match the characters you typed. The new version runs every query through Apple's on-device Foundation Models first — the same Apple Intelligence stack that powers Writing Tools — and turns it into a structured search intent before hitting the Apple Music API. Everything happens on the Mac itself: no cloud round-trip, nothing leaves the machine, and it works on a plane.

What that buys in practice:

- **Misspellings resolve to the artist you meant.** "tailor swift" finds Taylor Swift instead of an empty result list.
- **Conversational queries work.** "that new sabrina carpenter album" or "works by chopin" get parsed into artist, term, and category rather than searched literally.
- **Vibe queries get re-ranked.** Ask for "chill piano for working" and the model reorders and filters the results to match the mood, dropping the obvious mismatches.
- **Artist-scoped queries route correctly.** "beatles albums" browses albums by The Beatles; "popular songs by queen" biases toward top tracks.

If Apple Intelligence isn't available (or the model stumbles), everything falls back to the old deterministic parser, so worst case you get exactly the search you had before.

## Keeping a nondeterministic model honest

The scary part of shipping an LLM in the search path is that a tiny prompt change can quietly break it. At one point the parser started mangling artist names — "brahms" came back as "braves" — which is the kind of regression a unit test can't catch, because the model is nondeterministic and only runs against live Apple Intelligence.

The answer was **IntentBench**, a benchmark harness that compiles the app's real parser sources against the live on-device model and measures whether artist names survive the round trip across repeated iterations. It can't run in CI, so `release.sh` now refuses to ship until it has been run by hand.

Full disclosure from this release's run: 139 of 140 name-survival checks passed, and query routing was perfect at 65 of 65. Composer names — the soft spot last time out, where "brahms piano" had a habit of coming back as Mahler — survived 60 of 60. I won't pretend to know exactly why: the parser's prompt learned classical catalogue numbers this cycle, and the underlying Apple model drifts on its own schedule. The benchmark measures the result, not the cause.

The one miss was a pop star rather than a composer: "doja cat" came back as "Dua Cat" in one iteration out of five. Worth being clear about what that costs, because I'd been telling myself otherwise — the deterministic parser is a fallback for when the model *fails*, not for when it's confidently wrong. A mangled name sails straight through and searches for the wrong artist. That's precisely why the benchmark is a release gate and not a box I ticked once.

This release also fixes the subtlest bug of the batch: the parser used one long-lived model session, which accumulates a transcript across every search. After enough searches it overflows the model's context window, every request starts throwing, and the app silently falls back to dumb search until relaunch. Now each parse gets a fresh session, with a prewarmed spare standing by so the first search stays fast.

## Classical music, rendered like it deserves

I'm a classical nerd, and nothing dates a music app faster than watching it truncate "Die Entführung aus dem Serail, K. 384: Overture" into one ellipsized line as if it were a three-word pop title. Apple Music crams classical titles into a `Work, Catalogue: Movement` string, so MiniMusic has been quietly growing a formatter that takes them back apart — and this release rounds it out.

The now-playing view splits the title so the work gets its own line and the catalogue number and movement stay intact together underneath:

> **Die Entführung aus dem Serail**
> K. 384: Overture
> W. A. Mozart · Academy of St Martin in the Fields

That split isn't a dumb break on punctuation. The formatter knows the real catalogue systems — Köchel numbers for Mozart, BWV for Bach, Hoboken for Haydn, RV for Vivaldi, D. for Schubert, S. for Liszt, WoO, HWV, and a couple dozen more — and it won't split inside a parenthetical, so "(after RV 117)" stays put. Spelled-out keys become proper accidentals along the way: "C-Sharp Minor" renders as C♯ Minor, "A-Flat Major" as A♭ Major.

Composers get the same care. The credit line shows *composer · performer*, with given names abbreviated to initials the way a concert program would: Wolfgang Amadeus Mozart becomes W. A. Mozart, Jean-Philippe Rameau becomes J.-P. Rameau — but Ludwig *van* Beethoven keeps his particle and Ralph Vaughan Williams keeps his whole surname. New in this release: a credit listing several composers ("Percy Grainger & Edvard Grieg") abbreviates each name instead of giving up ("P. Grainger & E. Grieg"). And when a queue entry arrives without composer or movement metadata, the app refreshes it from the catalog so the classical treatment doesn't silently disappear.

Unlike the AI search, all of this is deterministic string handling with a proper unit test suite — Köchel numbers don't need a neural network, they need somebody to care.

## Quality of life

A few smaller things rode along:

- **Search history.** The app now keeps a log of each search's query and what it returned — how many results per category, whether the AI parser handled it, and the top matches.
- **Rotating search prompts.** The empty search field cycles through example queries, which doubles as discoverability for the new natural-language powers.
- **"Playing from" context.** The player shows where the current track came from — a playlist, an album, a search.
- **Long result lists scroll** instead of growing the panel off the bottom of the screen.

As with the [macOS 27 update](/posts/2026-06-18-updating-minimusic-for-macos-27/), I built this with [Claude Code](https://claude.ai/code) doing the heavy lifting. The code is [on GitHub](https://github.com/danielhopkins/MiniMusic), and the release is up now — existing installs will pick it up through Sparkle.
