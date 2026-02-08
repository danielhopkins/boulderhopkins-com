---
title: "About"
layout: "single"
url: /about/
summary: "About Dan Hopkins"
ShowReadingTime: false
ShowBreadCrumbs: false
ShowPostNavLinks: false
ShowToc: false
hidemeta: true
---

{{< rawhtml >}}
<style>
  /* Hide the default PaperMod post title */
  .post-header { display: none; }

  /* ---- About Page Styles ---- */
  .about {
    --accent: #305B90;
    --accent-soft: rgba(48, 91, 144, 0.12);
    --accent-softer: rgba(48, 91, 144, 0.06);
    text-align: center;
    padding: 1rem 0 2rem;
  }

  [data-theme="dark"] .about {
    --accent: #6FA3D8;
    --accent-soft: rgba(111, 163, 216, 0.15);
    --accent-softer: rgba(111, 163, 216, 0.06);
  }

  /* ---- Photo ---- */
  .about__photo-wrap {
    position: relative;
    display: inline-block;
    margin-bottom: 2rem;
    animation: aboutFadeUp 0.8s ease both;
  }

  .about__photo {
    width: 220px;
    height: 220px;
    border-radius: 50%;
    object-fit: cover;
    display: block;
    box-shadow:
      0 4px 24px rgba(0, 0, 0, 0.08),
      0 1px 4px rgba(0, 0, 0, 0.04);
    border: 3px solid var(--theme);
    outline: 2px solid var(--accent);
    outline-offset: 3px;
    transition: transform 0.4s ease, box-shadow 0.4s ease;
  }

  .about__photo:hover {
    transform: scale(1.03);
    box-shadow:
      0 8px 32px rgba(0, 0, 0, 0.12),
      0 2px 8px rgba(0, 0, 0, 0.06);
  }

  /* ---- Name ---- */
  .about__name {
    font-family: 'Cormorant Garamond', Georgia, serif;
    font-size: 3rem;
    font-weight: 600;
    color: var(--primary);
    line-height: 1.1;
    margin: 0 0 0.75rem;
    letter-spacing: -0.01em;
    animation: aboutFadeUp 0.8s ease 0.1s both;
  }

  /* ---- Title & Location ---- */
  .about__role {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 1.05rem;
    font-weight: 400;
    color: var(--secondary);
    margin: 0 0 0.3rem;
    animation: aboutFadeUp 0.8s ease 0.2s both;
  }

  .about__role a {
    color: var(--accent);
    text-decoration: none;
    font-weight: 500;
    transition: opacity 0.2s;
  }

  .about__role a:hover {
    opacity: 0.75;
  }

  .about__location {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.9rem;
    font-weight: 300;
    color: var(--secondary);
    letter-spacing: 0.08em;
    text-transform: uppercase;
    margin: 0 0 2.5rem;
    animation: aboutFadeUp 0.8s ease 0.3s both;
  }

  /* ---- Divider ---- */
  .about__divider {
    width: 48px;
    height: 2px;
    background: var(--accent);
    border: none;
    margin: 0 auto 2.5rem;
    opacity: 0.5;
    animation: aboutFadeUp 0.8s ease 0.35s both;
  }

  /* ---- Tagline ---- */
  .about__tagline {
    font-family: 'Cormorant Garamond', Georgia, serif;
    font-size: 1.55rem;
    font-style: italic;
    font-weight: 400;
    color: var(--content);
    line-height: 1.5;
    max-width: 480px;
    margin: 0 auto 3rem;
    animation: aboutFadeUp 0.8s ease 0.4s both;
  }

  /* ---- Experience ---- */
  .about__experience {
    max-width: 520px;
    margin: 0 auto 3rem;
    text-align: left;
    animation: aboutFadeUp 0.8s ease 0.45s both;
  }

  .about__exp-item {
    display: flex;
    gap: 1rem;
    padding: 1rem 0;
    border-bottom: 1px solid var(--accent-soft);
  }

  .about__exp-item:last-child {
    border-bottom: none;
  }

  .about__exp-logo {
    width: 40px;
    height: 40px;
    border-radius: 8px;
    object-fit: contain;
    flex-shrink: 0;
    background: var(--accent-softer);
    padding: 4px;
  }

  .about__exp-details {
    flex: 1;
    min-width: 0;
  }

  .about__exp-title {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.95rem;
    font-weight: 600;
    color: var(--primary);
    margin: 0;
    line-height: 1.3;
  }

  .about__exp-company {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.88rem;
    font-weight: 400;
    color: var(--secondary);
    margin: 0.1rem 0 0;
  }

  .about__exp-date {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.78rem;
    font-weight: 300;
    color: var(--secondary);
    margin: 0.2rem 0 0;
  }

  .about__exp-desc {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.85rem;
    font-weight: 400;
    color: var(--content);
    margin: 0.4rem 0 0;
    line-height: 1.45;
    font-style: italic;
  }

  /* ---- Interests ---- */
  .about__section-label {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.72rem;
    font-weight: 500;
    color: var(--secondary);
    letter-spacing: 0.14em;
    text-transform: uppercase;
    margin: 0 0 1rem;
    animation: aboutFadeUp 0.8s ease 0.5s both;
  }

  .about__tags {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 0.6rem;
    margin-bottom: 3rem;
    animation: aboutFadeUp 0.8s ease 0.55s both;
  }

  .about__tag {
    font-family: 'Libre Franklin', sans-serif;
    font-size: 0.85rem;
    font-weight: 400;
    color: var(--primary);
    background: var(--accent-softer);
    border: 1px solid var(--accent-soft);
    border-radius: 100px;
    padding: 0.45em 1.1em;
    text-decoration: none;
    transition: background 0.25s, border-color 0.25s, transform 0.25s;
  }

  .about__tag:hover {
    background: var(--accent-soft);
    border-color: var(--accent);
    transform: translateY(-1px);
  }

  a.about__tag {
    cursor: pointer;
  }

  a.about__tag:hover {
    color: var(--accent);
  }

  /* ---- Social ---- */
  .about__social {
    display: flex;
    justify-content: center;
    gap: 0.5rem;
    animation: aboutFadeUp 0.8s ease 0.65s both;
  }

  .about__social-link {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 48px;
    height: 48px;
    border-radius: 50%;
    color: var(--secondary);
    background: transparent;
    transition: color 0.3s, background 0.3s, transform 0.3s;
  }

  .about__social-link:hover {
    color: var(--accent);
    background: var(--accent-soft);
    transform: translateY(-2px);
  }

  .about__social-link svg {
    width: 22px;
    height: 22px;
    fill: currentColor;
  }

  /* ---- Animations ---- */
  @keyframes aboutFadeUp {
    from {
      opacity: 0;
      transform: translateY(16px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  /* ---- Responsive ---- */
  @media (max-width: 480px) {
    .about__name { font-size: 2.4rem; }
    .about__tagline { font-size: 1.3rem; }
    .about__photo { width: 180px; height: 180px; }
  }
</style>

<div class="about">
  <div class="about__photo-wrap">
    <img class="about__photo" src="/images/profile-large.png" alt="Dan Hopkins">
  </div>

  <h1 class="about__name">Dan Hopkins</h1>

  <p class="about__role">People and business focused engineering leader at <a href="https://www.stackhawk.com">StackHawk</a></p>
  <p class="about__location">Boulder, Colorado</p>

  <hr class="about__divider">

  <p class="about__tagline">Developing teams, then software.</p>

  <p class="about__section-label">Experience</p>
  <div class="about__experience">
    <div class="about__exp-item">
      <div class="about__exp-details">
        <p class="about__exp-title">VP of Engineering</p>
        <p class="about__exp-company">StackHawk</p>
        <p class="about__exp-date">Oct 2022 – Present</p>
        <p class="about__exp-desc">Leading an incredible group of engineers in a high growth startup to disrupt the security tooling world.</p>
      </div>
    </div>
    <div class="about__exp-item">
      <div class="about__exp-details">
        <p class="about__exp-title">Director, Engineering</p>
        <p class="about__exp-company">Splunk</p>
        <p class="about__exp-date">Jun 2018 – Oct 2022</p>
        <p class="about__exp-desc">Continuing the battle to make on-call suck less.</p>
      </div>
    </div>
    <div class="about__exp-item">
      <div class="about__exp-details">
        <p class="about__exp-title">VP of Engineering</p>
        <p class="about__exp-company">VictorOps</p>
        <p class="about__exp-date">Mar 2013 – Jun 2018</p>
        <p class="about__exp-desc">Built the software. Built the team.</p>
      </div>
    </div>
    <div class="about__exp-item">
      <div class="about__exp-details">
        <p class="about__exp-title">Technical Lead, User Acquisition</p>
        <p class="about__exp-company">LivingSocial</p>
        <p class="about__exp-date">May 2011 – Mar 2013</p>
        <p class="about__exp-desc">Leading team of 7 developers to create RESTful services in Scala and web sites in Ruby for 100 million active subscribers.</p>
      </div>
    </div>
    <div class="about__exp-item">
      <div class="about__exp-details">
        <p class="about__exp-title">Enterprise Content Management Architect</p>
        <p class="about__exp-company">Zia Consulting</p>
        <p class="about__exp-date">Dec 2007 – May 2011</p>
        <p class="about__exp-desc">Architect and technical lead for small teams building Java and Flex based applications.</p>
      </div>
    </div>
  </div>

  <p class="about__section-label">Interests</p>
  <div class="about__tags">
    <span class="about__tag">Application Security</span>
    <span class="about__tag">Agentic Development</span>
    <a class="about__tag" href="https://margothopkins.com" target="_blank" rel="noopener noreferrer">Hanging with My Kid</a>
    <span class="about__tag">Skiing with My Amazing Wife</span>
    <a class="about__tag" href="https://github.com/danielhopkins/forscore-cli" target="_blank" rel="noopener noreferrer">Piano</a>
    <a class="about__tag" href="https://copta.org/board-of-directors/" target="_blank" rel="noopener noreferrer">Public Education</a>
  </div>

  <p class="about__section-label">Connect</p>
  <div class="about__social">
    <a class="about__social-link" href="https://github.com/danielhopkins" target="_blank" rel="noopener noreferrer" title="GitHub">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/></svg>
    </a>
    <a class="about__social-link" href="https://www.linkedin.com/in/bolderdanh" target="_blank" rel="noopener noreferrer" title="LinkedIn">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 01-2.063-2.065 2.064 2.064 0 112.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
    </a>
    <a class="about__social-link" href="https://twitter.com/boulderDanH" target="_blank" rel="noopener noreferrer" title="X / Twitter">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M18.901 1.153h3.68l-8.04 9.19L24 22.846h-7.406l-5.8-7.584-6.638 7.584H.474l8.6-9.83L0 1.154h7.594l5.243 6.932ZM17.61 20.644h2.039L6.486 3.24H4.298Z"/></svg>
    </a>
    <a class="about__social-link" href="https://www.facebook.com/boulderDanielHopkins" target="_blank" rel="noopener noreferrer" title="Facebook">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/></svg>
    </a>
  </div>
</div>
{{< /rawhtml >}}
