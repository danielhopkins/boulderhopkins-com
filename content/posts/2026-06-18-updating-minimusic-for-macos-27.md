---
date: '2026-06-18T11:00:00-06:00'
draft: false
title: 'Updating MiniMusic for macOS 27'
thumbnail: /images/minimusic-icon.png
tags:
    - macos
    - swift
    - swiftui
    - music
---

[MiniMusic](https://github.com/danielhopkins/MiniMusic) is a little menu bar app I built to scratch my own itch: a fast, keyboard-driven way to control Apple Music without leaving whatever I'm working on. Hit a global hotkey, a dropdown appears under the menu bar icon, search my library or the catalog, hit return, and the music plays. It lives in the menu bar, ships updates through [Sparkle](https://sparkle-project.org/), and otherwise stays out of the way.

I upgraded to macOS 27 (Golden Gate) and two things were suddenly broken: the global hotkey did nothing, and album artwork stopped showing up. What I expected to be a quick afternoon of patches turned into a genuinely interesting tour of how SwiftUI's menu bar support has aged. I worked through it with [Claude Code](https://claude.ai/code), which was especially useful here because a lot of the debugging was empirical—build, run, observe, repeat—rather than something you can reason out from the docs.

## The hotkey that did nothing

The headline bug: pressing the hotkey did absolutely nothing. No flicker, no window, no error.

The first surprise was that the shortcut had drifted to **Ctrl+Space**, which macOS reserves for switching input sources. The system was eating the keystroke before my app ever saw it. That alone would explain "nothing happens." Migrated it back to Option+Space and added a guard so a known-bad binding gets fixed automatically.

But the more interesting discovery came when I confirmed the handler *was* firing on the correct key and the window *still* didn't open. It turns out **SwiftUI's `MenuBarExtra` can't be opened programmatically on macOS 27.** I tried every angle:

- The status item's button has no target/action anymore, so `performClick()` and `sendAction()` do nothing.
- A synthetic mouse click works—but only from a process with Accessibility permission, which a menu bar utility shouldn't need.
- And the popover's content window doesn't even exist until a *real* click creates it, so there's nothing to order forward.

In other words, `MenuBarExtra` in `.window` mode is designed to be opened by the user clicking it, full stop. There's no supported hook to open it from a global hotkey.

The fix was to stop fighting it and go back to AppKit: an app-managed `NSStatusItem` plus an `NSPanel` that hosts the same SwiftUI views. Opening is now just `makeKeyAndOrderFront`—completely reliable, no permissions, no synthetic events. As a bonus, the panel is a real key window, which fixed a subtler problem: in the popover, pressing Return in the search field would close the whole thing instead of searching, because the popover never truly became key.

## Missing album art

The second bug was simpler but a classic MusicKit gotcha. I'd been loading artwork with SwiftUI's `AsyncImage` and a URL from `artwork.url(...)`. That works fine for catalog content, but for items in your **library**, MusicKit hands back a non-HTTP `musicKit://` URL that `AsyncImage` has no idea how to load—so it silently fell back to a placeholder.

The right tool is MusicKit's own `ArtworkImage`, which knows how to render both. One catch: it's a fixed-size view, so a naive `.frame(height:)` doesn't constrain it and the artwork happily expanded to swallow the entire window. Wrapping it in a fixed-size container and clipping fixed that.

## A few more papercuts

The new SDK's stricter Swift 6 concurrency checking flagged a real data race—a non-`Sendable` `ApplicationMusicPlayer.Queue` crossing a `Task` boundary—that had been quietly fine before. And because the app now manages its own windows, I had to hand-roll the Settings window too, since SwiftUI's `Settings` scene won't display from an agent (menu-bar-only) app.

The most cautionary moment had nothing to do with macOS 27: when I went to cut the release, I discovered the **Sparkle signing key was gone**, lost with an old computer. That key is what existing installs use to trust updates, and it can't be regenerated—a new key won't match what's already deployed. I rotated to a fresh key (and *immediately backed it up this time*), which means anyone on an older build has to grab the new version manually once before auto-updates resume. Lesson very much learned.

## Shipping it

[MiniMusic 26.618.1](https://github.com/danielhopkins/MiniMusic/releases) is out, working on macOS 27, and the code is [on GitHub](https://github.com/danielhopkins/MiniMusic) if you want to poke at it. The broader lesson I keep relearning: SwiftUI is wonderful right up until you need fine-grained control over a platform-specific surface like the menu bar—at which point a thin layer of AppKit underneath is still the most reliable answer.
