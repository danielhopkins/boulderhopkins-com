---
title: "Applications"
layout: "single"
url: /applications/
summary: "Applications and tools by Dan Hopkins"
ShowReadingTime: false
ShowBreadCrumbs: false
ShowPostNavLinks: false
ShowToc: false
hidemeta: true
hideTitle: true
---

{{< rawhtml >}}
<div class="apps">
  <div class="apps__header">
    <h1 class="apps__title">Applications</h1>
    <p class="apps__subtitle">Open-source tools and apps I've built</p>
  </div>

  <div class="apps__grid">

    <div class="apps__card">
      <img class="apps__icon" src="/images/meld-icon.png" alt="Meld icon">
      <div class="apps__info">
        <h2 class="apps__name">Meld</h2>
        <p class="apps__platform">iOS &middot; TestFlight beta</p>
        <p class="apps__desc">
          A score keeper for card and board games. Set up individuals or teams, high or low score wins, an optional target, and only the columns the game needs — bids, trump suits, tallies, notes — then score rounds on a shared scorepad. Players and columns can change mid-game, house rules pin to the top of the board, and finished games are archived with a winner. Includes a built-in reference for 20 classic games, from Spades and Euchre to Cribbage and Nerts.
        </p>
        <div class="apps__tags">
          <span class="apps__tag">Swift</span>
          <span class="apps__tag">SwiftUI</span>
          <span class="apps__tag">iOS 26+</span>
        </div>
        <div class="apps__links">
          <a class="apps__link" href="https://testflight.apple.com/join/FxQT6A5C" target="_blank" rel="noopener noreferrer">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M19 9h-4V3H9v6H5l7 7 7-7zM5 18v2h14v-2H5z"/></svg>
            Join the beta
          </a>
          <a class="apps__link" href="/posts/2026-07-21-meld-a-score-keeper-for-game-night/">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8zm0 2 4 4h-4zM8 13h8v2H8zm0 4h8v2H8zm0-8h3v2H8z"/></svg>
            Read the announcement
          </a>
        </div>
      </div>
      <div class="apps__screenshot-wrap">
        <img class="apps__screenshot" src="/images/meld-board.png" alt="Meld screenshot showing the scorepad for a game of Spades">
      </div>
    </div>

    <div class="apps__card">
      <img class="apps__icon" src="/images/minimusic-icon.png" alt="MiniMusic icon">
      <div class="apps__info">
        <h2 class="apps__name">MiniMusic</h2>
        <p class="apps__platform">macOS</p>
        <p class="apps__desc">
          A lightweight menu bar player for Apple Music. Control playback, search your library and the catalog, manage your queue, and browse playlists — all from a small popover window. Features a global hotkey, keyboard navigation, and smart search that checks your library and Apple Music in parallel.
        </p>
        <div class="apps__tags">
          <span class="apps__tag">Swift</span>
          <span class="apps__tag">SwiftUI</span>
          <span class="apps__tag">MusicKit</span>
          <span class="apps__tag">macOS 26+</span>
        </div>
        <div class="apps__links">
          <a class="apps__link" href="https://github.com/danielhopkins/MiniMusic" target="_blank" rel="noopener noreferrer">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/></svg>
            GitHub
          </a>
          <a class="apps__link" href="https://github.com/danielhopkins/MiniMusic/releases" target="_blank" rel="noopener noreferrer">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M19 9h-4V3H9v6H5l7 7 7-7zM5 18v2h14v-2H5z"/></svg>
            Download
          </a>
        </div>
      </div>
      <div class="apps__screenshot-wrap">
        <div class="apps__shots">
          <img class="apps__screenshot apps__screenshot--alpha" src="/images/minimusic-screenshot.png" alt="MiniMusic screenshot showing the menu bar player with playback controls over the album artwork">
          <img class="apps__screenshot apps__screenshot--alpha" src="/images/minimusic-search.png" alt="MiniMusic screenshot showing search results for a Miles Davis album">
          <img class="apps__screenshot apps__screenshot--alpha" src="/images/minimusic-queue.png" alt="MiniMusic screenshot showing the play queue with composer credits">
        </div>
      </div>
    </div>

    <div class="apps__card">
      <img class="apps__icon" src="/images/forscore-cli-icon.png" alt="forscore-cli icon">
      <div class="apps__info">
        <h2 class="apps__name">forscore-cli</h2>
        <p class="apps__platform">macOS &middot; Command Line</p>
        <p class="apps__desc">
          A command-line tool for managing <a href="https://forscore.co" target="_blank" rel="noopener noreferrer">forScore</a> metadata on macOS. Reads the local forScore iCloud database to let you search, organize, and manage your sheet music library from the terminal. Installable via Homebrew.
        </p>
        <div class="apps__tags">
          <span class="apps__tag">Rust</span>
          <span class="apps__tag">Homebrew</span>
          <span class="apps__tag">iCloud</span>
        </div>
        <div class="apps__links">
          <a class="apps__link" href="https://github.com/danielhopkins/forscore-cli" target="_blank" rel="noopener noreferrer">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/></svg>
            GitHub
          </a>
        </div>
      </div>
    </div>

  </div>
</div>
{{< /rawhtml >}}
