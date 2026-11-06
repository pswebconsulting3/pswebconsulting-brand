# PS Web Consulting — Design System

Version 1.0 · The standing rules for how we build.

**Two layers.** Structure is fixed and applies to every project including client work: spacing, scale, motion physics, layout, quality floor. Brand is swapped per project: colour, typeface, radius personality, signature. The PS Web brand values live in this system; a client project replaces that block and keeps the rest.

Source of truth is `tokens.css` and the `PS Brand` collection in Figma. This document explains the reasoning. Where they disagree, the code wins.

---

## 1. Colour

### The ratio rule

This is the most important rule in the system and the one most often broken.

**Roughly 90% of any surface is black, white and grey. About 10% is the accent.**

Magenta is not a decorative colour. It marks something — a problem found, an action requested, the current state. If it appears on every card, badge and heading it stops meaning anything and the page reads as loud rather than confident. When it's rare, one magenta element pulls the eye exactly where you want it.

Practical test: screenshot a page, blur it, and count the magenta. More than one obvious area per screenful is too much.

### Semantics

The two accents are a pair with meaning, not a palette.

| | Colour | Means |
|---|---|---|
| Primary | Magenta `#ef13f2` | The mark. Problems found, actions requested, the current state. |
| Secondary | Cyan `#05f2db` | The fix. After-state, success, confirmed, resolved. |

This maps directly onto how we sell: we mark what's wrong, then show it fixed. Anywhere the two appear together, the meaning should be legible without explanation.

### Atmosphere — the third category

The ratio rule governs **accents**. Atmosphere is different and is not counted against it.

Atmosphere is diffuse coloured light sitting behind content: card glows, section washes, ambient corners. It can use many hues at once without becoming clutter, but only under six conditions. Break any one and the section fragments.

1. **One lightness, one chroma. Hue is the only variable.** `oklch(62% 0.20 H)`. Generated in hex or HSL, the hues drift in apparent brightness and some cards shout louder than others. This is the rule that does most of the work.
2. **The hue arc is bounded: 328° to 185°.** Magenta to cyan, through violet, indigo, blue and azure. Nothing warm, nothing green. The palette is literally the space between our two brand colours, so it cannot wander off-brand.
3. **Opacity ceiling 16%.** Atmosphere is a ground, not a fill. If you can name the colour at a glance from across the room, it's too strong.
4. **Never behind text.** It sits at `z-index: -1`, anchored away from the heading and body copy, so contrast stays measured against pure black.
5. **Same anchor position across a set.** Consistent placement is half of why differently-coloured cards still read as one component.
6. **One saturated element per card, identical on all of them.** Hue variety lives only in the diffuse layer. The moment a card gets a button matching its own glow, the set falls apart.

Hue assignment is by meaning — service family, content category — never at random. Reference implementation in `atmosphere.html`.

### Atmosphere applications

Three sanctioned uses. Anything else needs a reason.

**Card glow** — inside a panel, anchored to the same corner across the set, `z-index: -1`. One hue per card assigned by category.

**Halo** — the same light outside a panel, centred behind it. Used to lift a single important element off the page: a contact panel, a pricing card, a closing CTA. One per page, or it stops meaning "this one".

**Arc numerals** — a numbered sequence where the numeral is a three-stop gradient at fixed chroma, and the hue rotates from 328° at the first step to 185° at the last. Magenta is the problem, cyan is resolved, so the colour becomes the progress indicator. Bloom is a `drop-shadow` on the glyph, not a `text-shadow` on the box, capped at 28% so the edges stay crisp. Only valid where the content is genuinely a sequence — the same test as numbered markers.

Reference implementation for the last two in `numerals-and-halo.html`.

### Forms

Fields are dark: `--color-surface-2` with a hairline border. **Never white inputs on a dark ground** — a white field is a 21:1 slab that outweighs the submit button and breaks the page's hierarchy. The brightest thing in a form must be the primary action.

Primary submit is magenta with black text. Focus moves the border to magenta and lifts the field background one step. Confirmation and reassurance text uses cyan, because it describes the resolved state.

### Palette

| Token | Value | Use |
|---|---|---|
| `--color-bg` | `#000000` | Page ground |
| `--color-bg-2` | `#08080b` | Alternate band |
| `--color-surface` | `#131318` | Cards, panels |
| `--color-line` | `#26262f` | Borders, dividers, rules |
| `--color-ink` | `#ffffff` | Primary text |
| `--color-ink-muted` | `#a8a8b6` | Secondary text |
| `--color-ink-faint` | `#6e6e80` | Micro labels, metadata |
| `--color-accent` | `#ef13f2` | Primary. 18px and above only. |
| `--color-accent-bright` | `#ff4df5` | Required for magenta text below 18px |
| `--color-accent-ink` | `#000000` | Text on a magenta fill |
| `--color-fix` | `#05f2db` | Secondary. Safe at any size. |
| `--color-field-deep` | `#013baf` | Background field, never an accent |

### Contrast, verified

| Combination | Ratio | Verdict |
|---|---|---|
| White on black | 21:1 | — |
| Cyan on black | 14.7:1 | Safe at any size |
| Bright magenta `#ff4df5` on black | 7.6:1 | Safe at any size |
| Base magenta on black | 6.05:1 | 18px+ or 14px bold only |
| Black on magenta | 6.05:1 | Correct for buttons |
| **White on magenta** | **3.47:1** | **Fails. Never.** |
| **White on `#447ef2`** | **3.8:1** | **Fails for body copy** |

Buttons use black text on magenta. This surprises people and it's correct — white fails.

---

## 2. Typography

**Montserrat, one family, every role.** Deliberate: clean, self-hosts easily, already deployed.

The tradeoff is that it will not make us distinctive on its own. **The treatment has to.**

### Weight contrast is the tool

Jump from Black (900) to Regular (400) with nothing filling the middle. A page set in Bold and Regular looks like a template; a page that drops from 900 at display size to 400 at body size looks designed. Mix weights inside a single headline — that's the move that makes one family look intentional.

### Tracking is the second tool

Display type tightens to −4.5%. Micro labels and buttons open to +14% to +18% uppercase. That range is itself a signature.

### The ramp

| Style | Weight | Size token | Line | Tracking |
|---|---|---|---|---|
| Display / Hero | 900 | `--text-5xl` | .94 | −4.5% |
| Display / Section | 900 | `--text-3xl` | 1.0 | −3.5% |
| Heading L | 700 | `--text-2xl` | 1.12 | −3% |
| Heading M | 700 | `--text-xl` | 1.25 | −1.5% |
| Heading S | 600 | `--text-lg` | 1.35 | −.5% |
| Lead | 400 | `--text-lg` | 1.55 | 0 |
| Body | 400 | `--text-base` | 1.6 | 0 |
| Small | 400 | `--text-sm` | 1.6 | 0 |
| Micro label | 700 | `--text-xs` | 1.2 | +18%, uppercase |
| Button | 800 | `--text-sm` | 1 | +14%, uppercase |

**Rules:** body copy caps at 66 characters, display at 16. Headings get `text-wrap: balance`, paragraphs get `text-wrap: pretty`. Never a typeface that isn't Montserrat.

**Loading:** self-host through the child theme with `font-display: swap` and a `preload` on the display weight. Not the Google Fonts CDN.

---

## 3. Layout and spacing

4px base unit, 8px rhythm. Every value comes from the `--space-*` scale. If a gap needs a value that isn't on the scale, the layout is wrong, not the scale.

Related things sit close, unrelated things sit far apart. Use one step or three steps of difference, never one and a half — a 16px and a 20px gap teach the reader nothing.

Containers: 1360px standard, 720px reading, 1600px wide. Mobile-first, breakpoints at 480 / 768 / 1024 / 1280 / 1536.

**Do not centre everything.** A page where every section is a centred heading, centred paragraph and centred button is the default AI layout. Centre the display type; vary the alignment and column split below it, with a reason each time.

---

## 4. Motion

### Physics

All durations from `--dur-*`, all easings from `--ease-*`. No bespoke `cubic-bezier`, no `0.3s` typed inline.

Duration scales with distance. A state change is 170ms. An element entering the viewport is 520ms. A hero sequence is 900ms. A 500ms hover feels broken; a 170ms scroll reveal is invisible.

### Composition

**One orchestrated moment per page**, normally the hero: four or five elements arriving in a deliberate order, staggered ~90ms. Everything below the fold is quiet by comparison. Thirty independent fade-ins is not motion design, it's a tic — and it's the clearest tell of an AI-generated page.

Reveal distance stays at 26px. Elements travelling 100px look like a template.

### Performance

Animate `transform`/`translate`, `scale`, `rotate` and `opacity`. Nothing else. `top`, `left`, `width`, `height` and `margin` cost frames on the mid-range Android phones that are half of most clients' traffic.

### Accessibility

`prefers-reduced-motion` ships on every project and is tested, not assumed. It's in `tokens.css` under `@layer base`.

### Libraries

GSAP with ScrollTrigger for orchestrated and scroll-driven work. Native CSS transitions for hover, focus and simple state. Scroll-driven CSS animations behind `@supports (animation-timeline: view())` with the IntersectionObserver as the fallback — the system ships both. Lenis only when smooth scroll is a deliberate part of the direction. Never three animation libraries on one site.

---

## 5. Technical standards (2026)

These are what "modern" actually means now. They are requirements, not preferences.

### CSS architecture

- **`@layer`** — the system declares `tokens, base, components, utilities, overrides`. This is what stops an Elementor widget or a plugin from out-specifying the system. Any third-party CSS lands unlayered and therefore *wins* by default, so plugin styles get wrapped into `overrides` deliberately rather than fought with `!important`.
- **Logical properties** — `padding-inline`, `margin-block`, `inline-size`. Costs nothing, makes the system RTL-ready.
- **`color-mix()`** for derived colours rather than hardcoded tints. One source value, every variation computed.
- **`@property`** for typed custom properties so gradients and percentages can actually animate.
- **Container queries** for components. A card should respond to its own width, not the viewport — that's what makes it reusable in a sidebar and a full-width band without a variant.
- **`:has()`** for parent state instead of JavaScript class toggles.
- **Subgrid** for aligning rows across independent cards.

### Performance budget

The metric that matters is **INP**, not FID. Targets on every site we ship:

| Metric | Target |
|---|---|
| LCP | under 2.0s |
| INP | under 200ms |
| CLS | under 0.1 |
| Total JS | under 150KB compressed |

Practices: `fetchpriority="high"` on the LCP image and no lazy-load on it; explicit `width`/`height` on every image; AVIF with WebP fallback; `content-visibility: auto` on below-fold sections; self-hosted fonts with `preload` on the display weight only; Speculation Rules for prerendering likely next pages.

### Accessibility — WCAG 2.2 AA is the floor

- Visible focus that is not obscured by sticky headers (`scroll-margin-block-start` is in the base layer for this)
- Target size 24px minimum; we use 44px
- Contrast 4.5:1 body, 3:1 large text and UI borders
- No drag-only interactions without a keyboard alternative — the before/after comparator uses a real `input[type=range]` for exactly this reason
- Accessible authentication: no cognitive puzzle to log in
- One `h1` per page, no skipped heading levels
- Real alt text on content images, empty alt on decorative

### WordPress and Elementor

- Tokens load through the **child theme**, never Elementor's Custom CSS box
- Token values are mirrored into **Elementor Global Colors and Global Fonts** so client-facing editor controls stay usable
- Hero sections and any orchestrated motion go in as HTML widgets with the system's own markup
- Standard content sections are built natively so the client can still edit them
- Never Elementor Motion Effects and GSAP on the same element
- Disable Elementor's Google Fonts loading; we self-host

---

## 6. The signature

Every project has one element it is remembered by, drawn from the client's own world. Everything around it stays quiet so it has room to land.

**Ours is the redline.** We open by marking up the site a prospect already has. The brand device and the sales pitch are the same thing: magenta annotation marks over real work, resolving to a cyan fixed state. It appears on the site, in proposals, in case studies, in decks.

The rule that makes it work: **one signature moment per page.** The previous gradient orbs failed partly because they were everywhere, competing with the content and with each other.

---

## 7. Voice

Clients are business owners, not marketers. They care whether the phone rings.

**Use:** concrete outcomes over adjectives, real numbers, the audit framing, active voice, sentence case, plain verbs.

**We are:** diagnostic, dependable, direct.
**We are not:** playful, disruptive, luxury.

**Banned:** cutting-edge, leverage, solutions, seamless, elevate, transform your online presence, unlock, empower, best-in-class.

The test: if a sentence could appear unchanged on any other agency's site, delete it.

Controls name what happens — "Send message", not "Submit". An action keeps the same word through the whole flow: the button that says Publish produces a message that says Published. Errors say what went wrong and how to fix it. Empty states say what to do next.

---

## 8. Anti-generic rules

These exist because the default output of every AI design tool converges on the same handful of looks.

- No cream background with a serif display and a terracotta accent
- No near-black with a single acid-green accent and nothing else
- No gradient blobs behind the hero
- No glassmorphism as a default surface treatment
- No icon-plus-heading-plus-two-lines grid unless the content genuinely is three parallel things
- Numbered markers (01 / 02 / 03) only when the content is a real sequence
- More than one accent competing for attention
- Every section centred
- Stock photography where a real client screenshot would do
- **The WordPress-agency default** — blue gradient, icon grid, stock smiles. That is the thing we sell clients an escape from.

Every project takes one deliberate aesthetic risk, and we can say what it is and why.

---

## 9. Per-project setup

For each client, create a small brand source and attach it alongside this system:

```
client-name/
  brand.css      — the BRAND block from tokens.css, filled in
  BRIEF.md       — goal, audience, page jobs, signature, radius
                   personality, motion direction, the one risk
  references/    — 3-5 screenshots with a note on what to take
                   from each, and what NOT to take
  content/       — real copy, real names, real numbers
```

Never brief with adjectives alone. "Modern and clean" produces the default. Reference images plus real content produce something specific.
