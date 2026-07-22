# boulder dan — boulderhopkins.com

Personal tech blog of Dan Hopkins. Hugo + the **PaperMod** theme (a git submodule —
never edit `themes/PaperMod/`; override into `layouts/` or `assets/css/extended/`).

Deployed on Vercel from `main`.

```bash
hugo server            # local preview at :1313
hugo --gc              # production build into public/
```

---

## The design system

The site has a real design system: **boulder dan**, exported from Claude Design.
Its narrative reference — brand story, visual foundations, iconography, and the
writing voice — is checked in at **`docs/design-system.md`**. Read it before any
design work.

The machine-readable half lives in **`assets/css/extended/00-tokens.css`**. That
file is the single source of truth for color, type, spacing, elevation and motion.

### The one hard rule

**Never write a raw hex, font stack, or magic pixel value into a stylesheet.**
Use a token. If the value you need doesn't exist, add the token to
`00-tokens.css` first, then use it. A stray `#305B90` is a bug even though it
renders correctly — it's a second source of truth.

```css
/* no */                          /* yes */
color: #305B90;                   color: var(--bd-accent);
font-family: 'Cormorant Garamond', Georgia, serif;   font-family: var(--font-display);
font-size: 2.8rem;                font-size: var(--fs-title);
border-bottom: 1px solid rgba(48, 91, 144, 0.12);    border-bottom: var(--border-hairline);
```

PaperMod's `theme-vars.css` already owns the neutral ramp (`--theme`, `--entry`,
`--primary`, `--secondary`, `--content`, `--border`) and the container widths
(`--nav-width` 1024px, `--main-width` 720px, `--gap`, `--radius`), and flips them
for dark mode. `00-tokens.css` is the brand layer on top and deliberately does
not repeat them.

### Stylesheet load order

Hugo concatenates `assets/css/extended/*.css` in **sorted filename order**, after
PaperMod's core. The prefixes encode the layering:

| File | Layer |
| --- | --- |
| `00-tokens.css` | tokens — must load first |
| `editorial.css` | global: home, post list, post single, tags |
| `header.css` | masthead + site footer |
| `page-*.css` | one per standalone page — must load **after** `editorial.css` |
| `post-hero.css` | opt-in post cover hero |

---

## Adding a new standalone page

Standalone pages (`/about`, `/applications`, `/speaking`) are hand-built HTML in
a `rawhtml` shortcode, styled from a dedicated `page-*.css`. Follow that pattern.

**1. Front matter** — `hideTitle` suppresses PaperMod's stock post header so the
page can render its own serif heading:

```yaml
---
title: "Reading"
layout: "single"
url: /reading/
summary: "What I'm reading"
ShowReadingTime: false
ShowBreadCrumbs: false
ShowPostNavLinks: false
ShowToc: false
hidemeta: true
hideTitle: true
---
```

**2. Markup** — semantic HTML in `{{< rawhtml >}}`, BEM-ish classes namespaced to
one block (`.reading`, `.reading__item`). **No `<style>` blocks and no `style=""`
attributes in content files** — that's what the old pages did and it duplicated
the whole token set three times.

**3. Styles** — a new `assets/css/extended/page-reading.css`. The `page-` prefix
is load-bearing: it sorts after `editorial.css`, which is what lets a page rule
win a specificity tie against the global prose rules.

**4. Register** the page in `hugo.yaml` under `menu.main` if it needs a nav item.

### Two specificity traps — read this before styling images or links

Standalone pages render *inside* `.post-content`, and PaperMod styles everything
in the reading column as prose. Those rules **outrank a single component class**,
so components silently lose. Both traps were live bugs on this site.

**Images.** `.post-content img { border-radius: 4px; margin: 1rem 0 }` (0,1,1)
beats `.reading__cover` (0,1,0). Prefix the block and the element:

```css
.reading img.reading__cover {   /* (0,2,1) — wins */
  border-radius: var(--radius-icon);
  margin: 0;
}
```

**Links.** `editorial.css` gives links in the reading column an accent underline
plus a hover wash. That's correct for inline prose links, wrong for pills, icon
buttons and heading links. Opt components out:

```css
.reading a.reading__pill {
  box-shadow: none;
}
.reading a.reading__pill:hover {   /* must also out-specify .post-content a:hover */
  background: var(--bd-accent-soft);
}
```

Verify with computed styles rather than by eye — a 4px radius where you wanted
22px is easy to miss in a screenshot.

---

## Design rules that are easy to get wrong

- **One accent.** `--bd-accent` blue is the only saturated UI hue. The Summit
  palette (`--bd-sun`, `--bd-forest*`, `--bd-sky-*`) is for brand moments — the
  mark, favicons, cover treatments — **never** a page background or gradient.
- **Hairlines, not boxes.** A "card" in a list is an entry with a bottom
  `--border-hairline` that washes `--bd-accent-softer` on hover. No border box,
  no elevation. App cards on `/applications` are the one sanctioned surface.
- **Shadows are rare.** Only the round About photo, the medal bars, app icons and
  screenshots. All chrome is shadowless.
- **One entrance.** `bdFadeUp` — 16px rise, `--dur-enter`, staggered ~0.1s down a
  list. Never invent a second keyframe; there used to be four identical ones.
  **Always** wrap it in `@media (prefers-reduced-motion: no-preference)`.
- **Radii.** `--radius-sm` 4px, `--radius` 8px, `--radius-logo` 12px,
  `--radius-card` 16px, `--radius-thumb` 20px, `--radius-icon` 22px,
  `--radius-pill` 100px.
- **No emoji anywhere**, in UI or prose. Personality comes from the serif type,
  the monogram and the medal bars. `→` `·` `»` are used typographically.
- **No transparency or blur.** Tints are solid rgba washes of the accent.
- Every change must work in **both themes** — check `[data-theme="dark"]`.

## Post covers

A post with a `cover` image gets PaperMod's in-column figure at `--radius`, which
is what the system's foundations call for. `cover: { hero: true }` opts into the
full-bleed hero in `post-hero.css` instead.

Use the hero only for a **wide photograph with quiet space along the bottom**.
It mangles an illustration that carries its own text or a centered composition —
the 340px crop cuts the artwork and light areas swallow the reversed meta line.
Both current cover posts are illustrations, so neither opts in. Check the render
before setting it.

## Brand mark

The **"Summit"** monogram — a Colorado dawn scene (layered sky, glowing sun,
snow-capped Flatirons, foreground pine). It replaced an earlier flat "bd"
letter-square.

`static/favicon.svg` is the **single source of truth**. It is used in two places:

1. **The icon set** — `favicon.ico`, `favicon-16/32x32.png`, `apple-touch-icon.png`
   and `safari-pinned-tab.svg` are all generated from it.
2. **The masthead lockup** — `site.Params.label.icon` in `hugo.yaml` points
   straight at it, so the header mark sits beside the italic wordmark at 28px
   (24px under 640px). Styling is in `header.css` under "Brand lockup".

If the mark changes, regenerate the whole icon set. `apple-touch-icon.png` must
be **squared and opaque** (iOS applies its own mask); the favicons keep the
14/64 rounded corners; `safari-pinned-tab.svg` must be flat black on transparent.

Note the masthead lockup goes slightly beyond the design system — its `NavHeader`
component is wordmark-only. The monogram there was an explicit call.

## Writing voice

Full detail in `docs/design-system.md` — the short version:

- First person singular. Warm, candid, senior-but-not-lecturing. Address the
  reader as "you" when giving advice.
- Confident without hype; comfortable admitting uncertainty. Occasional dry humor.
- **Specificity carries it** — real numbers and names ("$0 → $10M ARR",
  "139 of 140 checks passed") beat abstractions.
- Sentence case in prose, Title Case in titles, UPPERCASE wide-tracked for UI
  meta. The brand name is always lowercase: "boulder dan".
- Em-dashes for asides, `→` for growth, curly quotes.
- AI is a first-class subject, discussed like a practitioner — neither breathless
  nor dismissive.
