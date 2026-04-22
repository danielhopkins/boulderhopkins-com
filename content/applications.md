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
---

{{< rawhtml >}}
<style>
  .post-header { display: none; }

  .apps {
    --accent: #305B90;
    --accent-soft: rgba(48, 91, 144, 0.12);
    --accent-softer: rgba(48, 91, 144, 0.06);
    padding: 0.5rem 0 2rem;
  }

  [data-theme="dark"] .apps {
    --accent: #6FA3D8;
    --accent-soft: rgba(111, 163, 216, 0.15);
    --accent-softer: rgba(111, 163, 216, 0.06);
  }

  /* ---- Page Header ---- */
  .apps__header {
    text-align: center;
    margin-bottom: 3rem;
    animation: appsFadeUp 0.8s ease both;
  }

  .apps .apps__title {
    font-family: 'Cormorant Garamond', Georgia, serif;
    font-size: 2.8rem;
    font-weight: 600;
    color: var(--primary);
    line-height: 1.1;
    margin: 0 0 0.6rem;
    letter-spacing: -0.01em;
  }

  .apps__subtitle {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 1rem;
    font-weight: 300;
    color: var(--secondary);
    margin: 0;
  }

  /* ---- Cards Grid ---- */
  .apps__grid {
    display: flex;
    flex-direction: column;
    gap: 2rem;
  }

  /* ---- Card ---- */
  .apps__card {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 1.8rem;
    align-items: start;
    padding: 2rem;
    border-radius: 16px;
    background: var(--entry);
    border: 1px solid var(--border);
    transition: border-color 0.3s, box-shadow 0.3s, transform 0.3s;
  }

  .apps__card:nth-child(1) { animation: appsFadeUp 0.8s ease 0.15s both; }
  .apps__card:nth-child(2) { animation: appsFadeUp 0.8s ease 0.3s both; }

  .apps__card:hover {
    border-color: var(--accent-soft);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.06), 0 2px 8px rgba(0, 0, 0, 0.03);
    transform: translateY(-2px);
  }

  [data-theme="dark"] .apps__card:hover {
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2), 0 2px 8px rgba(0, 0, 0, 0.1);
  }

  /* ---- Icon ---- */
  .apps__icon {
    width: 96px;
    height: 96px;
    border-radius: 22px;
    object-fit: cover;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
    flex-shrink: 0;
  }

  /* ---- Card Content ---- */
  .apps__info {
    min-width: 0;
  }

  .apps__name {
    font-family: 'Cormorant Garamond', Georgia, serif;
    font-size: 1.7rem;
    font-weight: 600;
    color: var(--primary);
    margin: 0 0 0.15rem;
    line-height: 1.2;
  }

  .apps__platform {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.75rem;
    font-weight: 500;
    color: var(--secondary);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin: 0 0 0.75rem;
  }

  .apps__desc {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.95rem;
    font-weight: 300;
    color: var(--content);
    line-height: 1.65;
    margin: 0 0 1.2rem;
  }

  /* ---- Tags ---- */
  .apps__tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.45rem;
    margin-bottom: 1.2rem;
  }

  .apps__tag {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.72rem;
    font-weight: 400;
    color: var(--secondary);
    background: var(--accent-softer);
    border: 1px solid var(--accent-soft);
    border-radius: 100px;
    padding: 0.3em 0.85em;
  }

  /* ---- Links ---- */
  .apps__links {
    display: flex;
    gap: 0.75rem;
    flex-wrap: wrap;
  }

  .apps__link {
    display: inline-flex;
    align-items: center;
    gap: 0.45rem;
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.85rem;
    font-weight: 500;
    color: var(--accent);
    text-decoration: none;
    padding: 0.45em 1em;
    border-radius: 100px;
    border: 1px solid var(--accent-soft);
    background: transparent;
    transition: background 0.25s, border-color 0.25s, transform 0.25s;
  }

  .apps__link:hover {
    background: var(--accent-soft);
    border-color: var(--accent);
    transform: translateY(-1px);
  }

  .apps__link svg {
    width: 16px;
    height: 16px;
    fill: currentColor;
    flex-shrink: 0;
  }

  /* ---- Animations ---- */
  @keyframes appsFadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  /* ---- Screenshot ---- */
  .apps__screenshot-wrap {
    grid-column: 1 / -1;
    margin-top: 0.5rem;
  }

  .apps__screenshot {
    width: 260px;
    border-radius: 12px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
    border: 1px solid var(--border);
    transition: transform 0.3s ease;
  }

  [data-theme="dark"] .apps__screenshot {
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  }

  .apps__screenshot:hover {
    transform: scale(1.02);
  }

  /* ---- Responsive ---- */
  @media (max-width: 640px) {
    .apps .apps__title { font-size: 2.2rem; }
  }
  @media (max-width: 540px) {
    .apps__card {
      grid-template-columns: 1fr;
      text-align: center;
      justify-items: center;
    }

    .apps__tags { justify-content: center; }
    .apps__links { justify-content: center; }
    .apps__screenshot { width: 220px; }
  }
</style>

<div class="apps">
  <div class="apps__header">
    <h1 class="apps__title">Applications</h1>
    <p class="apps__subtitle">Open-source tools and apps I've built</p>
  </div>

  <div class="apps__grid">

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
        <img class="apps__screenshot" src="/images/minimusic-screenshot.png" alt="MiniMusic screenshot showing the menu bar player">
      </div>
    </div>

    <div class="apps__card">
      <img class="apps__icon" src="/images/forscore-cli-icon.png" alt="forscore-cli icon">
      <div class="apps__info">
        <h2 class="apps__name">forscore-cli</h2>
        <p class="apps__platform">macOS &middot; Command Line</p>
        <p class="apps__desc">
          A command-line tool for managing <a href="https://forscore.co" target="_blank" rel="noopener noreferrer" style="color: var(--accent); text-decoration: none;">forScore</a> metadata on macOS. Reads the local forScore iCloud database to let you search, organize, and manage your sheet music library from the terminal. Installable via Homebrew.
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
