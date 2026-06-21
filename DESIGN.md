# myku.studio — design notes

Salient spec, distilled from the original design handoff (`Myku.dc.html`). The
live page is `index.html`; this is the reference behind it.

## Intent
A single-screen "threshold" landing page for **myku.studio** — the landing page
for the *myku* workbench app (a place to reflect, distil, and make sense of the
noise). Deliberately *not* a marketing/SaaS page: no features, pricing,
testimonials, or persuasion. The visitor arrives, reads a haiku, and may step
through to the sister site **myku.space**. Governing principles: reduction,
generous negative space (Ma), restrained craft. The composition should always
feel ~80–90% empty. No scroll — it fits one viewport.

## Layout
- **Page background**: flat, nearly-white warm field. No texture.
- **Centered paper sheet** floats in the field: warm off-white, subtle paper
  grain, soft drop shadow, 2px corner radius, centered both axes.
  - Desktop sheet: **642 × 766px** (in 1440×860, ~48px top/bottom breathing room).
  - Mobile sheet: **314 × 670px** (in 390×844, ~88px top/bottom, ~38px sides).
  - The real page is **one responsive composition** that scales between these
    two anchor layouts (breakpoint at 640px).
- **Inside the sheet** (vertically + horizontally centered column):
  1. Wordmark `· myku ·`.
  2. Haiku, 50px below wordmark (desktop) / 38px (mobile). Centered as a group,
     but the three lines are **left-aligned to each other**.
  3. Footer link line, absolutely pinned to the sheet bottom (54px padding
     desktop / 44px mobile), horizontally centered. A large empty void sits
     between haiku and footer — **the void is the point.**

## Components
**Wordmark `· myku ·`** — flex row, `gap: 0.28em`. Word "myku": serif,
weight 500, `letter-spacing: 0.1em`, `line-height: 1`, warm charcoal. Flanking
mid-dots: weight 400, **cinnabar** (artist's-seal red). Size 56/38px (desktop/mobile).

**Haiku** — exact copy (capitalization/punctuation exact):
```
Workshop in waiting,
empty space full of meaning.
Myku painted sign.
```
Serif, **italic**, weight 400, `letter-spacing: 0.012em`, muted warm gray.
27px / line-height 1 / gap 15px (desktop); 20px / line-height 1.1 / gap 12px (mobile).

**Footer link line** — centered flex row, gap 15/12px:
- `MYKU·SPACE` → `https://myku.space`. Rendered `myku` + **cinnabar mid-dot** +
  `space`, uppercased via CSS (the dot replaces the literal period; stays
  cinnabar even on hover).
- `| APP STORE` → **hidden by default** (gated on `APP_STORE_LIVE`, default
  false; app not yet in the App Store). When live, reveal a faint `|` separator
  + `App Store` link to the real store URL.
- Link text: `Helvetica, Arial, sans-serif`, weight 400, uppercase, no underline.
  10.5px / `letter-spacing: 0.28em` / `padding-bottom: 7px` (desktop);
  9.5px / `0.24em` / 6px (mobile). Underline is a transparent-at-rest
  `box-shadow: 0 1px 0`.

**Paper grain** — full-bleed overlay inside the sheet, `mix-blend-mode: multiply`,
`opacity: 0.2`. Tiling fractal-noise SVG (`feTurbulence`, 180×180 tile,
`baseFrequency 0.9`, `numOctaves 2`, `stitchTiles="stitch"`) as a `background-image`
data URI. `pointer-events: none`.

**Ink blot** (on in the final design) — soft dark organic stain lower-left + a
tiny satellite splatter, both `mix-blend-mode: multiply`, blue-black ink.
- Main: `42×37px` at `bottom:20%; left:13%`, opacity 0.52, organic shape
  `border-radius: 47% 53% 67% 33% / 60% 45% 55% 40%`.
- Splatter: `8×7px` circle at `bottom:17%; left:20.5%`, opacity 0.42.
- Mobile: blot `27×24px` at `bottom:19%; left:13%`; splatter `6×5px` at
  `bottom:16.5%; left:21%`.
- (Coffee ring, torn corner, crease blemishes existed in the prototype but are
  **off** in the final design.)

## Interactions
- **Hover is on the whole sheet** (parent-hover-child, *not* on the small links):
  - Sheet lifts: `transform: translateY(-3px)`, shadow deepens, `transition: 0.6s ease`.
  - Every footer link lifts toward ink (`transition: 0.4s ease`).
  - Pointing at a specific link darkens just that one further and fades in a
    **cinnabar underline** (`box-shadow: 0 1px 0`) — signalling which door
    you're about to step through. The cinnabar mid-dot never changes.
- **Entrance**: content group and footer each do a one-shot upward rise on load
  (`translateY(8px) → 0`, ~1.7s ease; footer delayed 0.25s). **Opacity is not
  animated** — content rests fully visible; never gate visibility on the animation.
- No other motion, no gradients, no productivity-app UI language.

## Tokens
Colors (oklch, ~hex):
| role | oklch | ~hex |
|---|---|---|
| Page background (no texture) | `0.987 0.0015 100` | `#fbfbfa` |
| Paper sheet (warm) | `0.966 0.008 80` | `#f4f1ea` |
| Wordmark / strong ink | `0.3 0.012 65` | `#322e29` |
| Haiku / secondary ink | `0.46 0.01 65` | `#56524c` |
| Cinnabar accent | `0.5 0.165 33` | `#b5462f` |
| Link resting | `0.62 0.009 65` | `#7a766f` |
| Link sheet-hover (all) | `0.4 0.012 62` | `#4c4842` |
| Link pointed-at | `0.27 0.02 55` | `#312b24` |
| Link hover underline | `0.55 0.16 33` | `#c1543c` |
| Footer separator `\|` | `0.79 0.006 70` | `#c4c0b9` |
| Ink blot | `0.3 0.035 258` | `#2a2f3d` |

Typography: **Cormorant Garamond** (serif, 400/500 + italic 400) for wordmark &
haiku; **Helvetica, Arial, sans-serif** for footer links.

Shadows:
- Rest: `0 1px 2px rgba(45,40,33,0.05), 0 14px 34px rgba(45,40,33,0.075)`
- Hover: `0 3px 6px rgba(45,40,33,0.06), 0 24px 54px rgba(45,40,33,0.12)`

Final defaults shipped: Cormorant Garamond · Warm paper · texture opacity 0.2 ·
ink blot **on** · App Store link **off** · coffee ring / torn corner / crease **off**.

## Links
- `MYKU·SPACE` → `https://myku.space`
- `App Store` → real store URL when live (currently `#`, hidden)
