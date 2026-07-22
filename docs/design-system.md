# boulder dan — Design System

The design system for **boulder dan** ([boulderhopkins.com](https://boulderhopkins.com)) — the personal tech blog of Dan Hopkins: a CTO / VP-Engineering who writes about building, leading, and tinkering, with a strong AI-native bent. Recent posts cover shipping on-device LLMs in a Mac app, doubling engineering output with Claude Code, and how to stop an AI from misleading you with false "balance."

The look is **editorial**: a serif/sans pairing (Cormorant Garamond + Libre Franklin), a single muted-blue accent, generous whitespace, hairline rules, uppercase wide-tracked meta, and one quiet fade-up on load. Playful yet sleek — a serif italic wordmark, a "bd" monogram, and little tri-stripe "service ribbon" medals on the About page.

## Sources

- **GitHub — site repo:** [`danielhopkins/boulderhopkins-com`](https://github.com/danielhopkins/boulderhopkins-com). A Hugo site on the **PaperMod** theme; the brand lives in `assets/css/extended/editorial.css` + `header.css` (layered over PaperMod's `theme-vars.css`), the fonts in `layouts/partials/extend_head.html`, and the About/Applications/Speaking pages in `content/`. Explore it to build higher-fidelity work against the real markup and content.
- Base theme: [PaperMod](https://github.com/adityatelange/hugo-PaperMod) — its surface/text tokens and container widths are preserved here so recreations match the live site.

> **Reader note:** you may not have access to the repo above; nothing assumes it. Everything needed is captured in this project.

## Fonts

Both are the site's **real** fonts (Google Fonts) — not substitutions:

- **Cormorant Garamond** — serif display. Headings, page/post titles, the italic wordmark, taglines.
- **Libre Franklin** — humanist sans. Body copy, nav, meta, UI, light lead text.

Loaded via `@import` in `tokens/fonts.css`. (No local `@font-face` binaries ship — the compiler will report "Fonts: 0"; consumers still get the faces from Google Fonts through the import.)

## Logo / brand mark

The site's brand mark is the **"Summit" monogram** — a Colorado dawn scene: a layered sky (deep blue → peach → gold), a glowing sun, snow-capped Flatirons ridgelines, and a foreground pine, in a rounded square or circle (`assets/favicon.svg`). It's the `Monogram` component, drawn as crisp vector from the `--bd-*` palette so it stays sharp from a 24px favicon to a hero lockup. It replaces the earlier flat "bd" letter-square, giving the identity the curiosity/exploration/nature personality the blog is about. The primary wordmark is "boulder dan" set in italic Cormorant Garamond (`Wordmark`).

---

## CONTENT FUNDAMENTALS

How the blog is written — match this voice in any generated copy.

- **Voice & person:** First person, singular. "I moved into management about ten years ago and figured my coding days were behind me." Warm, candid, senior-but-not-lecturing. Addresses the reader as "you" when giving advice ("You're not trying to get the AI to pick a side").
- **Tone:** Thoughtful and plainspoken. Confident without hype. Comfortable admitting uncertainty ("I won't pretend to know exactly why"). Occasional dry humor ("Köchel numbers don't need a neural network, they need somebody to care").
- **Casing:** Sentence case in prose. Titles are Title Case ("MiniMusic Learns to Listen: On-Device AI Search"). UI meta/nav is UPPERCASE with wide tracking. The wordmark and brand name are deliberately **lowercase** — "boulder dan".
- **Structure:** Short intro that states the point, then `## H2` sections with concrete sub-steps. Bulleted lists for parallel items. Often closes with a reflective or forward-looking line ("What a time to be alive.").
- **Specificity:** Real numbers and names carry the writing — "$0 → $10M ARR," "139 of 140 name-survival checks passed," "Coca-Cola, McDonald's, and ITV." Prefer a concrete example over an abstraction.
- **AI-forward framing:** AI is a first-class subject and tool, discussed like an experienced practitioner — neither breathless nor dismissive. Names tools directly (Claude Code, Apple Foundation Models).
- **Emoji:** **None.** The brand never uses emoji. Personality comes from serif type, the monogram, and the medal bars — not glyphs.
- **Punctuation:** Em-dashes for asides. Arrows (`→`) for growth/transformation. Curly quotes.

## VISUAL FOUNDATIONS

- **Color:** One saturated hue for UI — the editorial blue `--bd-accent` `#305B90` (lightens to `#6FA3D8` in dark mode). Everything else is PaperMod's neutral ramp (`--primary`, `--secondary`, `--content`) on white (`--theme`) / near-black (`rgb(29,30,32)`). The accent appears as links, active-nav underline, hairline rules, pill borders and soft washes (`--bd-accent-soft` 12%, `--bd-accent-softer` 6%). The **"Summit" mark palette** carries a warm counterpoint — dawn sky (`--bd-sky-top/-mid/-peach`), the Colorado sun `--bd-sun` `#F2B950` (also aliased `--accent-warm` for occasional warm highlights), and the nature greens `--bd-forest`/`-deep`/`-shadow` with cream snow `--bd-cream`. Keep blue as the single UI accent; reach for the sun/forest palette in brand moments (mark, thumbnail, cover treatments), never as gradient page backgrounds.
- **Type:** Serif display / sans body split (see Fonts). Large serif headings are tracked tight (`-0.015em`); meta and nav are uppercase, tracked wide (`0.14em`–`0.18em`). Light weight (300) for lead subcopy.
- **Backgrounds:** Flat theme color — no full-bleed hero images, no patterns, no gradients, no texture. Post cover images (when present) sit inside the reading column with `8px` radius. Photography is warm and real (Boulder parks, product screenshots, app icons); no stock-art gloss.
- **Layout:** Centered, single reading column — `--main-width` 720px for lists and articles, `--nav-width` 1024px for the masthead and footer. List pages and the home hero are centered; article bodies left-aligned within the column. Fixed elements: a hairline-bordered top masthead and a "back to top" affordance.
- **Borders & rules:** The system leans on **1px hairline borders** in `--bd-accent-soft` rather than boxes — between list entries, under the masthead, above footers, around pills. A signature short **48×2px centered accent rule** (`Divider`) separates editorial sections.
- **Cards:** There are no heavy drop-shadow cards. A "card" is a list entry separated by a bottom hairline that washes `--bd-accent-softer` on hover — no border box, no elevation. Radius `8px` where a surface is needed (code, cover images); pills are fully rounded (`100px`).
- **Shadows:** Reserved and soft — only the round About photo (`--shadow-photo`, plus a 2px accent outline ring) and the medal bars (`--shadow-medal`). UI chrome is shadowless.
- **Hover states:** Color shift to the accent + a faint `--bd-accent-softer` wash; pills and social buttons **lift** `translateY(-1px…-2px)`. ~0.2s ease. No scale on cards.
- **Press/active:** Return to resting transform (no shrink). The About photo scales up slightly (`1.03`) on hover as a deliberate exception.
- **Motion:** One entrance — `bdFadeUp`: opacity 0→1 with a 16px rise over 0.8s ease, staggered ~0.1s per item down a list. All motion is wrapped in `prefers-reduced-motion`. No bounces, no parallax, no attention-loops.
- **Transparency / blur:** None. Tints are solid-color rgba washes of the accent; no backdrop blur, no glass.
- **Corner radii:** `4px` small, `8px` cards/code, `12px` monogram square, `100px` pills.

## ICONOGRAPHY

- **Approach:** Sparse and functional. The site ships a small set of **inline SVG** glyphs (24×24, `stroke="currentColor"`, `stroke-width:2`, round caps/joins) — the classic Feather/Lucide silhouette. Used for: theme toggle (sun/moon), RSS, the external-link arrow, and the "back to top" caret.
- **Social:** Solid-fill brand glyphs (GitHub, LinkedIn, X, Facebook) as 24×24 SVG paths, rendered in circular buttons. The real paths are reproduced in `SocialLink` (`SOCIAL_ICONS`) — copied from the site's About page, not redrawn.
- **Company logos:** Real PNG logos live in `assets/logos/` (StackHawk, Splunk, VictorOps, LivingSocial, Zia), shown as small rounded contained thumbnails in `ExperienceItem`.
- **App icons / imagery:** Real product art in `assets/` — `minimusic-icon.png`, `meld-icon.png`, `forscore-cli-icon.png`, post covers (`balance-cover.png`, `stackhawk-cover.png`, `meld-board.png`), and Boulder photos.
- **Emoji / unicode as icons:** No emoji. Unicode is used typographically — `→` for growth, `·` `»` as separators, `♯`/`♭` accidentals in the app — never as decorative icons.
- **For new icons:** match the stroke set above; if you need more, pull from **[Lucide](https://lucide.dev)** (same 2px stroke, round caps) — the closest match to the site's hand-picked glyphs. Flag any substitution.

---

## Index / manifest

Root:
- `styles.css` — the single entry point (imports only). Consumers link this.
- `tokens/` — `fonts.css`, `colors.css`, `typography.css`, `spacing.css`, `elevation.css`.
- `thumbnail.html` — homepage tile.
- `SKILL.md` — Agent-Skills-compatible entry for using this system in Claude Code.
- `assets/` — favicon monogram, profile photo, company logos, app icons, post covers, Boulder photos.
- `guidelines/` — foundation specimen cards (Colors, Type, Spacing, Brand).

### Components (`components/`)
`window.BoulderDanDesignSystem_c97eee.*`

- **brand/** — `Wordmark` (italic serif lockup), `Monogram` (the "Summit" dawn-scene mark; square or circle).
- **core/** — `Button` (pill; primary/outline/ghost), `Tag` (chip pill), `Meta` (uppercase meta line), `Divider` (accent rule), `MedalBar` (tri-stripe company ribbon), `SocialLink` (round icon button), `CodeBlock` (fenced code with dark chrome + copy button).
- **content/** — `PostCard` (post-list entry), `SectionHeader` (editorial page header), `ExperienceItem` (work-history row), `AppCard` (applications showcase card).
- **navigation/** — `NavHeader` (the asymmetric editorial masthead), `SiteFooter` (powered-by footer), `PostNav` (prev/next post links).

### UI kits (`ui_kits/`)
- **blog/** — interactive recreation of the boulderhopkins.com blog: `HomeScreen`, `PostScreen` (with full-bleed cover-hero variant + inline `CodeBlock`s + `PostNav`), `TagsScreen`, `AboutScreen`, `AppsScreen`, wired through `index.html` (masthead nav, light/dark, router, footer). See `ui_kits/blog/README.md`.
- **applications/** — the `/applications` showcase page (`AppCard` grid for Meld, MiniMusic, forscore-cli); reuses the blog's `AppsScreen`.

### Intentional additions
Every component maps to a real element on the site; the identity was evolved with the user. `MedalBar` and `SocialLink` reproduce concrete on-page devices (About medals, About social row). `Monogram` is the "Summit" mark — a from-the-ground-up brand direction developed with the user to replace the flat "bd" square.
