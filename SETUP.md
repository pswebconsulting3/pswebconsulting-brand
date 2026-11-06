# Setting up the PS Web design system in Claude Design

Do this once. About 30 minutes.

---

## Step 1 — Put four files in your GitHub repo

Into `agency-house-baseline`:

```
tokens.css          the design system
DESIGN-SYSTEM.md    the rules and reasoning
landing-v2.html     the reference implementation
BRIEF-TEMPLATE.md   for client projects later
```

`landing-v2.html` matters most. Claude reads a working page far better than it reads a description of one, and that file is the system actually running — tokens, type ramp, atmosphere, motion, the signature.

To add files: github.com → your repo → **Add file** → **Upload files** → drag → **Commit changes**.

---

## Step 2 — Export the Figma file

Figma → **File** → **Save local copy** → you get a `.fig`.

It now carries 32 variables, 10 paint styles and 12 text styles, so the extraction will be clean rather than guesswork.

---

## Step 3 — Create the system

Claude Design → bottom of the window → **Design systems** → new.

**Company name and blurb:**

> PS Web Consulting: a WordPress web design and digital marketing agency in Schaumburg, Illinois. We open with a free 45-minute audit of a prospect's existing site, then rebuild it. This system is for our own material — proposals, decks, one-pagers, case studies and our website.

**Link code from GitHub:** paste the repo URL → **Add**. Wait for confirmation.

**Upload a .fig file:** drop the export from step 2.

**Add fonts, logos and assets:** your logo, light and dark.

**Any other notes:** paste everything in the box below.

---

## The notes text

> STRUCTURE comes from tokens.css: 4px spacing base, fluid type scale, motion durations and easings, component anatomy. Never invent spacing or timing values outside that scale.
>
> COLOUR has three categories and a hard ratio rule.
> — Neutrals (black, white, grey) are roughly 90% of any surface.
> — Magenta #ef13f2 is the primary accent and means "the mark on what is wrong": problems found, actions requested. It is rare. More than one obvious magenta area per screenful is wrong.
> — Cyan #05f2db means "the fixed state": after, success, confirmed, resolved.
> — Atmosphere is diffuse coloured light behind content and is NOT counted against the ratio. Generate it as oklch(63% 0.19 H) with hue bounded between 328 and 185 — magenta to cyan. Same lightness and chroma for every hue, opacity ceiling 0.15. Never behind text.
>
> CONTRAST is non-negotiable. Black text on magenta fills, never white — white on magenta is 3.47:1 and fails. Magenta text below 18px uses #ff4df5. Never small white text on #447ef2.
>
> TYPE is Montserrat only. Personality comes from weight contrast — jump from Black 900 to Regular 400 with nothing in the middle — and tracking contrast: −4.5% on display type, +18% uppercase on micro labels. Mix weights inside a single headline. Body copy caps at 66 characters. Multi-line uppercase headings need line-height 1.06 minimum, never 1.
>
> CSS: every clamp() must have whitespace around + and −. clamp(2rem, 1rem+4vw, 4rem) is invalid and silently falls back to browser defaults. Use @layer, logical properties, container queries for components, oklch for atmosphere, color-mix for derived tints.
>
> MOTION: one orchestrated moment per page, normally the hero, staggered ~90ms. Everything else quiet. Animate transform and opacity only. Always honour prefers-reduced-motion.
>
> SIGNATURE: the redline. Magenta annotation marks over a real screenshot resolving to a cyan fixed state. One signature moment per page.
>
> VOICE is plain, specific and evidence-led. Never use cutting-edge, leverage, solutions, seamless, elevate, unlock, empower, or transform your online presence. Controls name what happens — "Send message", not "Submit".
>
> AVOID: gradient blobs, glassmorphism, cream-and-terracotta, near-black-with-acid-green, icon-grid feature sections, centring every section, and the WordPress-agency default of blue gradients and stock photography.

---

## Step 4 — Review what it generates

Check three things:

1. Is magenta appearing everywhere, or sparingly? It should be rare.
2. Are the headings large, with a real jump down to body copy?
3. Is Montserrat the only typeface?

If any is wrong: organisation settings → your design system → **Open** → **Remix** → tell it what to change in plain language.

---

## Step 5 — Publish and test

Switch **Published** on.

Then create a project and prompt:

> A one-page proposal for a freight company website rebuild. Include the audit findings, scope, timeline and price.

You're checking whether the output looks like `landing-v2.html` in spirit — dark, restrained, one accent doing real work, big confident headings.

---

## Step 6 — Use it on one real thing

Your next proposal or pitch deck. Not a client site yet.

That tells you more in an hour than any further planning will. If it's good, we set up the second system for client work. If something's off, we fix it before it's baked into ten projects.

---

## Not yet — deliberately

Leave these alone until step 6 is done:

- The house baseline system for client sites
- The reference screenshot library
- The Elementor handoff kit
- Version tagging
- The two Figma layers with unloadable fonts (`1:1659`, `1:2352`)

All real, none urgent.
