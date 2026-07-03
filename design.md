# PavloDAR AI — Design System

> Parody AI startup that measures a fictional "Pavlodar Aura Score" from a name
> and a photo. The design sells nonsense with a straight face: bold monochrome
> editorial, Series-A confidence, restrained humor in the copy.
>
> Source of truth: `PavloDAR AI Landing Page/PavloDAR AI.dc.html` (Claude Design
> export), implemented as an Astro site in `src/`.

---

## 1. Brand

| Element | Value |
|---|---|
| Name | **PavloDAR AI** (wordmark rendered as `PAVLODAR`) |
| Model name | IrtyshNet™ v3.2 |
| Voice | Deadpan-serious startup copy; the joke is never winked at |
| Safety framing | Every surface repeats: aura scores are **fictional** — no cultural, ethnic, or geographic claims |

The wordmark is Anton, italic, `skewX(-6deg)`, uppercase — used at 22px in the
nav and at `min(19vw, 268px)` as the hero statement.

## 2. Typography

Loaded from Google Fonts (see `src/layouts/Layout.astro`):

| Role | Font | Usage |
|---|---|---|
| Wordmark | **Anton** (400, italic, skewed −6°) | `PAVLODAR` hero + nav brand, `--font-wordmark` |
| Display | **Bricolage Grotesque** (opsz 12–96, wght 400–800) | All headings, prices, stats, quotes — `--font-display` |
| Body | **Instrument Sans** (400/500/600) | Everything else — `--font-body` |

Scale:
- Section titles: `40px`, weight 700, letter-spacing `-0.02em` (30px on mobile)
- Gallery mega-title: `min(15.5vw, 220px)`, weight 500, line-height 0.94
- Eyebrows: `13px`, weight 600, uppercase, letter-spacing `0.08em`
- Body: `16px` / 1.55; captions and fineprint 12–14px

## 3. Color

Strict monochrome — no accent hue. Defined as CSS variables in
`src/styles/global.css`:

| Token | Value | Usage |
|---|---|---|
| `--black` | `#0B0B0B` | Ink, dark sections, Pro pricing card |
| `--white` | `#FFFFFF` | Page base, text on dark |
| `--gray-bg` | `#F4F4F2` | Pricing section, demo readout panel |
| `--muted` | `#8A8A8A` | Eyebrows on light, secondary text |
| `--muted-2` | `#9A9A9A` | Fineprint, legal |
| `--line-dark` | `rgba(255,255,255,0.15)` | 1px grid gaps / borders on dark |
| `--line-dark-strong` | `rgba(255,255,255,0.25)` | Nav borders, gallery rule |
| `--line-light` | `rgba(11,11,11,0.12)` | Card borders on light |

Section rhythm (top → bottom): dark hero → dark how-it-works → white demo →
gray pricing → white testimonials → **dark gallery → dark FAQ** → white footer.

## 4. Layout

- Content max-width: `1280px` (`--wrap`), side padding 48px (24px mobile)
- Cards: radius **24px** (panels, pricing), **20px** (grids, testimonials), **16px** (FAQ), inputs/buttons **10px**
- Signature pattern: **1px-gap grids** — cells of `--black` on a `--line-dark`
  background, radius on the container, `overflow: hidden` (how-it-works, FAQ, demo signals)
- Nav is `position: absolute` over the hero (not sticky): full-width strip,
  cells separated by 1px white borders, uppercase 13px labels

## 5. Hero

- `100vh` (min 720px), black
- **Background video**: two stacked `<video>` elements crossfade (0.8s opacity)
  through three 8s drone segments — `/hero-a.mp4`, `/hero-b.mp4`, `/hero-c.mp4`
  (Mashkhur Jusup Mosque at night, 720p). Next clip preloads on the hidden
  element; a 1s watchdog re-applies visibility if a fade ever misfires;
  autoplay retries on `canplay` and first `pointerdown` (iOS low-power mode)
- **Overlay**: uniform `rgba(0, 0, 0, 0.5)` between video and content
- Elements: giant wordmark (bottom-centered, `bottom: 6vh`), three stats
  top-right (1.2M / 94.7% / 37), circular 130px `PLAY DEMO` button
  (blurred glass: `rgba(255,255,255,0.12)` + `backdrop-filter: blur(6px)`)
- Modals: video ("Demo reel coming soon", 16:9, z-200) and methodology
  ("How IrtyshNet doesn't work", white card, z-100). Both close on backdrop
  click and Escape. The methodology modal currently has no visible trigger.

## 6. Sections & copy

| Section | Bg | Content |
|---|---|---|
| How it works (`#how`) | black | 3 steps: Upload a face / Enter a name / Receive your score |
| Demo (`#demo`) | white | Live scanner — see §7 |
| Pricing (`#pricing`) | gray | Free $0 / **Pro $29/mo (black card, featured)** / Enterprise Custom; features prefixed with `—` |
| Testimonials | white | 2 quotes, 38px initials avatars (black circle) |
| Gallery (`#trust`) | black | See §8 |
| FAQ (`#faq`) | black | 3 items, `<details>` accordion, +/− indicator |
| Footer | white | Brand + legal: "Aura scores are fictional. No cultural, ethnic, or geographic claims implied." |

## 7. Demo (the product)

Two panels: **intake** (bordered, white) and **readout** (gray, three stacked
states occupying the same grid cell).

Intake:
- Name input — optional; blank scans as **"Anonymous Subject"**
- Photo dropzone — optional; drag-drop or tap; preview rendered
  `grayscale(1) contrast(1.05)`; copy: "Photos are discarded immediately. We
  only store the vibe."
- Submit: full-width black button; label switches to "Analyzing..." while busy

States:
1. **Idle** — slow-spinning dashed circle, "Awaiting subject. The steppe is patient."
2. **Loading** — spinner + 4 messages cycling every 750ms
   (*Analyzing steppe resilience... / Checking Irtysh proximity... /
   Cross-referencing urban-vibe signals... / Finalizing aura composite...*);
   total scan time **2.6s**
3. **Result** — conic-gradient ring (black on 12% black) animating 0→score over
   1s with ease-out cubic + counting number; name; verdict line; Irtysh Index
   (`1.0–9.9 / 10`); Steppe Resilience (Emerging → Geological); 3 "detected
   signals" with percentages; "Scan again"

Scoring (deterministic — same inputs, same result, every time):
```
hash  = Σ (hash * 31 + charCode) >>> 0        // over trimmed name
score = 68 + hash % 30                        // 68–97, flatteringly high
```
Photo metadata (`name:size:lastModified`) seeds only the secondary readouts;
skipping the photo swaps the verdict to a "name resonance alone" line. This is
by design: **no real image analysis happens or is implied.**

## 8. Gallery — "They Trust Our App"

Reference: dark "Featured Work" editorial gallery.

- Black section; mega-title `They Trust / Our App` above a 1px rule
- **Vertical masonry**: CSS `columns: 3` (2 ≤1000px, 1 ≤640px), 40px gutters
- Images grayscale by default, **full color on hover** (0.35s)
- Caption per card: `NAME — note` (name 600 weight, note 60% white)
- Data lives in one array at the top of `src/components/Gallery.astro`;
  photos are flat files in `public/` (igor, skrip, imanbek, truwer, 104,
  cf38b75, jillzay, hakim, Aldik `.jpg`)

## 9. Motion

- `pd-fadeup` — 10px rise + fade; result card, modals
- `pd-spin` — spinner + idle dial (24s slow / 0.9s fast)
- Hero crossfade 0.8s ease; gallery hover de-grayscale 0.35s
- Hovers: solid buttons drop to 85% opacity; ghost/nav cells gain
  `rgba(255,255,255,0.1)` fill
- Everything respects `prefers-reduced-motion: reduce`

## 10. Components (Astro)

```
src/
  layouts/Layout.astro        fonts, meta, og tags
  styles/global.css           tokens, [hidden] fix, keyframes, .wrap/.eyebrow
  components/
    Nav.astro                 absolute bordered strip
    Hero.astro                video rotation, wordmark, stats, both modals
    HowItWorks.astro          1px-gap 3-col grid
    Demo.astro                scanner UI + scoring script
    Pricing.astro             3 plans, dark featured card
    Testimonials.astro        2 quote cards
    Gallery.astro             masonry "They Trust Our App"
    FAQ.astro                 details/summary accordion
    Footer.astro              brand + legal
  pages/index.astro           section order
```

## 11. Rules of the parody

1. Never imply real ethnicity/nationality/origin detection — the FAQ answers
   this explicitly with "No."
2. Scores are always flattering (68–97%); the product never insults the subject.
3. Copy is confident, specific, and absurd ("94.7% confidence in absolutely
   nothing") — humor lives in the words, never in the visual design.
4. Determinism is a feature: repeatable results make the fake feel "real"
   on stage.
