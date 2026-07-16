---
date: '2026-07-16T10:00:00-06:00'
draft: false
title: 'MiniMusic Learns to Listen: On-Device AI Search'
tags:
    - macos
    - swift
    - swiftui
    - music
    - ai
---

[MiniMusic](https://github.com/danielhopkins/MiniMusic) is my little menu bar app for controlling Apple Music from the keyboard: global hotkey, type, return, music. The new release, [MiniMusic 26.716.0](https://github.com/danielhopkins/MiniMusic/releases), is the biggest one yet, because the search box got a brain.

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

Full disclosure from this release's run: misspelling correction and query routing are at 100%, but the current model snapshot mangles a few composer names — "brahms piano" has a habit of coming back as Mahler, and Prokofiev sometimes turns into Puccini. Benchmarking the *previous* release's code showed the same failures, so it's drift in the underlying Apple model rather than anything in the app's prompt, and the deterministic fallback still finds these searches. It's a known soft spot I'll keep chasing.

This release also fixes the subtlest bug of the batch: the parser used one long-lived model session, which accumulates a transcript across every search. After enough searches it overflows the model's context window, every request starts throwing, and the app silently falls back to dumb search until relaunch. Now each parse gets a fresh session, with a prewarmed spare standing by so the first search stays fast.

## Quality of life

A few smaller things rode along:

- **Search history.** The app now keeps a log of each search's query and what it returned — how many results per category, whether the AI parser handled it, and the top matches.
- **Rotating search prompts.** The empty search field cycles through example queries, which doubles as discoverability for the new natural-language powers.
- **"Playing from" context.** The player shows where the current track came from — a playlist, an album, a search.
- **Tidier composer credits.** Multi-composer classical credits abbreviate each name instead of overflowing the line.
- **Long result lists scroll** instead of growing the panel off the bottom of the screen.

As with the [macOS 27 update](/posts/2026-06-18-updating-minimusic-for-macos-27/), I built this with [Claude Code](https://claude.ai/code) doing the heavy lifting. The code is [on GitHub](https://github.com/danielhopkins/MiniMusic), and the release is up now — existing installs will pick it up through Sparkle.
