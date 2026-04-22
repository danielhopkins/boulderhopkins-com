---
title: "Speaking"
layout: "single"
url: /speaking/
summary: "Conference talks, workshops, and podcast appearances by Dan Hopkins"
ShowReadingTime: false
ShowBreadCrumbs: false
ShowPostNavLinks: false
ShowToc: false
hidemeta: true
---

{{< rawhtml >}}
<style>
  .post-header { display: none; }

  .speaking {
    --accent: #305B90;
    --accent-soft: rgba(48, 91, 144, 0.12);
    --accent-softer: rgba(48, 91, 144, 0.06);
    padding: 0.5rem 0 2rem;
  }

  [data-theme="dark"] .speaking {
    --accent: #6FA3D8;
    --accent-soft: rgba(111, 163, 216, 0.15);
    --accent-softer: rgba(111, 163, 216, 0.06);
  }

  /* ---- Page Header ---- */
  .speaking__header {
    text-align: center;
    margin-bottom: 3rem;
    animation: speakingFadeUp 0.8s ease both;
  }

  .speaking .speaking__title {
    font-family: 'Cormorant Garamond', Georgia, serif;
    font-size: 2.8rem;
    font-weight: 600;
    color: var(--primary);
    line-height: 1.1;
    margin: 0 0 0.6rem;
    letter-spacing: -0.01em;
  }

  .speaking__subtitle {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 1rem;
    font-weight: 300;
    color: var(--secondary);
    letter-spacing: 0.04em;
    margin: 0;
  }

  /* ---- Item List ---- */
  .speaking__list {
    max-width: 640px;
    margin: 0 auto;
    animation: speakingFadeUp 0.8s ease 0.15s both;
  }

  .speaking__item {
    padding: 1.5rem 0;
    border-bottom: 1px solid var(--accent-soft);
  }

  .speaking__item:last-child {
    border-bottom: none;
  }

  .speaking__kind {
    display: inline-block;
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.68rem;
    font-weight: 600;
    color: var(--accent);
    background: var(--accent-softer);
    border: 1px solid var(--accent-soft);
    border-radius: 100px;
    padding: 0.2em 0.75em;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    margin: 0 0 0.6rem;
  }

  .speaking__item-title {
    font-family: 'Cormorant Garamond', Georgia, serif;
    font-size: 1.5rem;
    font-weight: 600;
    line-height: 1.25;
    margin: 0 0 0.4rem;
    letter-spacing: -0.005em;
  }

  .speaking__item-title a {
    color: var(--primary);
    text-decoration: none;
    transition: color 0.2s;
  }

  .speaking__item-title a:hover {
    color: var(--accent);
  }

  .speaking__venue {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.95rem;
    font-weight: 500;
    color: var(--primary);
    margin: 0;
  }

  .speaking__meta {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.82rem;
    font-weight: 300;
    color: var(--secondary);
    letter-spacing: 0.04em;
    margin: 0.2rem 0 0;
  }

  .speaking__desc {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.92rem;
    font-weight: 400;
    color: var(--content);
    margin: 0.7rem 0 0;
    line-height: 1.55;
  }

  /* ---- Animations ---- */
  @keyframes speakingFadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  /* ---- Responsive ---- */
  @media (max-width: 640px) {
    .speaking .speaking__title { font-size: 2.2rem; }
  }
  @media (max-width: 480px) {
    .speaking__item-title { font-size: 1.25rem; }
  }
</style>

<div class="speaking">
  <div class="speaking__header">
    <h1 class="speaking__title">Speaking</h1>
    <p class="speaking__subtitle">Conference talks, workshops, and podcast appearances</p>
  </div>

  <div class="speaking__list">

    <div class="speaking__item">
      <p class="speaking__kind">Workshop</p>
      <h2 class="speaking__item-title"><a href="https://www.linkedin.com/posts/stackhawk_dont-miss-dans-workshop-at-apidays-new-activity-7190771937072005120-nbtc/" target="_blank" rel="noopener noreferrer">Level Up Your Web &amp; API Security</a></h2>
      <p class="speaking__venue">apidays New York</p>
      <p class="speaking__meta">New York &middot; April 2024</p>
      <p class="speaking__desc">Hands-on workshop on finding and fixing web and API vulnerabilities, with a live walk-through of StackHawk against a real app.</p>
    </div>

    <div class="speaking__item">
      <p class="speaking__kind">Talk</p>
      <h2 class="speaking__item-title"><a href="https://developerweek2024.sched.com/event/1ZQOQ/virtual-open-talk-shifted-left-moving-from-a-reactive-to-proactive-mindset" target="_blank" rel="noopener noreferrer">Shifted Left: Moving from a Reactive to Proactive Mindset</a></h2>
      <p class="speaking__venue">DeveloperWeek 2024 &mdash; Virtual OPEN Expo Discovery Stage</p>
      <p class="speaking__meta">Virtual &middot; February 2024</p>
      <p class="speaking__desc">Tracing shift-left from its agile and DevOps roots to today, and what a proactive security mindset actually looks like in engineering practice.</p>
    </div>

    <div class="speaking__item">
      <p class="speaking__kind">Podcast</p>
      <h2 class="speaking__item-title"><a href="https://cloudfix.com/podcast/werner-vogels-reinvent-keynote-pulled-us-into-his-matrix-vision-of-the-future-live-from-las-vegas-with-stackhawk/" target="_blank" rel="noopener noreferrer">Werner Vogels' re:Invent Keynote Pulled Us Into His Matrix Vision of the Future</a></h2>
      <p class="speaking__venue">AWS Insiders &mdash; with Hilary Doyle &amp; Rahul Subramaniam</p>
      <p class="speaking__meta">Live from AWS re:Invent, Las Vegas &middot; December 2022</p>
      <p class="speaking__desc">Unpacking Werner Vogels' keynote on async, event-driven architecture and what shifting security left means for engineering teams.</p>
    </div>

    <div class="speaking__item">
      <p class="speaking__kind">Talk</p>
      <h2 class="speaking__item-title"><a href="https://speakerdeck.com/danielhopkins/actors-not-just-for-movies-anymore" target="_blank" rel="noopener noreferrer">Actors: Not Just for Movies Anymore</a></h2>
      <p class="speaking__venue">SCALE 13x &mdash; Southern California Linux Expo</p>
      <p class="speaking__meta">Los Angeles &middot; February 2015</p>
      <p class="speaking__desc">Why message-passing and the Actor model beat shared mutable state when clusters grow and 90s-era architectures start to crack.</p>
    </div>

  </div>
</div>
{{< /rawhtml >}}
