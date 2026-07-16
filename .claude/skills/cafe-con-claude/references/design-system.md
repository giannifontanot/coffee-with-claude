# Café con Claude — design system

Extracted from the real pages in this repository (`*.html`). Two registers coexist:
the **article register** (the vast majority of pages) and the minimalist **index
register** (`index.html`). The skill generates pages in the **article register**;
the index register is documented at the end for when you touch `index.html`.

---

## Article register (default — use this for new pages)

### Typography

Loaded from Google Fonts. This exact pairing appears on 40+ pages and *is* the
house style:

- **Display / headings — `Fraunces`, serif.** Variable weights 300–700 (+ italic
  axis), optical size axis pushed to display (`font-optical-sizing: 144`). Used
  for `h1`, `h2`, brand marks. The `h1` is **big and tight** (from
  `primero-invisible`): `font-size: clamp(42px, 8vw, 72px); line-height: 1.02;
  letter-spacing: -.018em; font-weight: 600;` — with `h1 em { font-style: italic;
  font-weight: 500; color: var(--rust); }` for the accented word.
- **Body — `Spectral`, serif.** Weights 400/500/600 plus italics. `line-height: 1.72`.
- **Eyebrows / labels / mono accents — `DM Mono`, monospace** (optional). Uppercase,
  wide letter-spacing.

Standard `<head>` load:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300..700;1,9..144,300..600&family=Spectral:ital,wght@0,400;0,500;0,600;1,400;1,500&display=swap" rel="stylesheet">
```

`Playfair Display` is an acceptable alternative display face on a few pages, but
prefer Fraunces + Spectral unless the user asks otherwise.

### Palette (the "cream / terracotta" system)

These are the most frequent hex values across the corpus. Put them in `:root`:

The current palette (from `primero-invisible-despues-imparable.html` — a warmer,
deeper cut of the cream/terracotta system). Put it in `:root`:

```css
:root {
  --bg:        #f2e8d6;  /* page background (paper) */
  --bg-deep:   #ecdfc8;
  --card:      #f9f3e6;  /* card & panel backgrounds */
  --card-2:    #f6efdf;
  --ink:       #2d261d;  /* primary text */
  --ink-soft:  #5b4f40;  /* secondary text */
  --ink-faint: #8a7c68;  /* faint text / numerals */
  --terracotta:#b04e29;  /* primary accent: links, rules, back-link */
  --rust:      #963c20;  /* deep accent / h1 <em> */
  --amber:     #cf9136;  /* secondary accent / highlights */
  --ochre:     #b9853a;
  --olive:     #6c7240;  /* tertiary accent */
  --olive-deep:#525830;
  --plum:      #6c3a4f;  /* tertiary accent */
  --plum-deep: #502635;
  --line:      #d9cdb8;  /* hairline borders on cards */
  --line-soft: rgba(45,38,29,.07);
  --shadow:    0 1px 2px rgba(45,38,29,.05), 0 14px 38px -22px rgba(45,38,29,.34);
  /* compatibility aliases for older component CSS */
  --cream:     #f2e8d6;
  --muted:     #5b4f40;
}
```

The paper is not flat: the body carries two soft radial gradients over `--bg`
(see the template's `body` rule). Optional extras seen in the wild:
`--steel #4a6b7a`, `--teal #3d7068`, `--cacao #8a5a3b` for multi-voice panels.
Accent tints are made with `rgba`, e.g. `rgba(176,78,41,0.06)` (terracotta wash)
for source boxes and hover states.

### Layout

- Centered single column. Article width `max-width: 820px` (long-form) or `640px`
  (tight essays). Wrap in `.wrap { max-width: 820px; margin: 0 auto; }`.
- `body`: the warm gradient paper over `--bg` (two `radial-gradient`s + `var(--bg)`),
  `color: var(--ink)`, `font-family: 'Spectral', Georgia, serif`, `line-height: 1.72`,
  `padding: 4.5rem 1.25rem 5rem` (top room for the fixed hamburger). Copy the
  exact `body` rule from `assets/template.html`.
- `html { scroll-behavior: smooth; }`

### Recurring components

- **Round hamburger + nav drawer / left rail** — a fixed **circular** button
  (`#nav-toggle`, `border-radius: 50%`, light `--card` face with a thin `--line`
  border) in the **top-left** corner; its three bars morph into an `✕` via an
  `.open` class (not a text swap). It toggles an off-canvas `nav.sidebar` that
  slides in from the left over a dim scrim. The drawer/rail (styled after
  `crecer es pelear` / `Episode 58`) holds the brand mark (`Café con Claude`,
  small-caps terracotta, plain — no emoji), an italic series sub-line, an `<ol>`
  table of contents whose links (`#s1`, `#s2`, …) point at the page's section ids
  (Fraunces numerals + title, a scroll-spy adds `.active` to the current one),
  and a `.rail-tools` foot with a `← Índice` home link. This is the site's
  standard navigation for any multi-section piece — see `la-panza-que-habla.html`,
  `primero-invisible-despues-imparable.html`.
  The drawer is **responsive**: on desktop (`min-width: 1000px`) it docks as a
  fixed left rail — always visible, no overlay — with the hamburger and scrim
  hidden and the article column shifted right (re-centring past `1500px`); below
  `1000px` it goes off-canvas and the top-left `☰` hamburger slides it in over a
  scrim. Keep the breakpoint low (1000px, not the 1180px some older pages use):
  laptops under OS display-scaling report a CSS width far below their physical
  pixels (a 1366px screen at 125% is ~1093 CSS px), so a high breakpoint leaves
  real desktops stuck on the collapsed hamburger. Never leave it collapsed on
  desktop.
- **Reading progress bar** — fixed, full-width, 3px, gradient of the accents,
  `z-index:100`. Width driven by scroll in JS. Common but optional.
- **Back control (top)** — a link-styled control at the very top. See SKILL.md:
  in this skill it is a `<button>` that runs `history.back()`, NOT an `<a href>`.
  Visual style is the classic `.home-link`: small, terracotta text with a
  terracotta bottom-border, `← ` glyph prefix, `margin-bottom: 2.2rem`.
- **Header** — optional `eyebrow`/`kicker` in **Spectral italic** (15px,
  terracotta, `letter-spacing: .06em`, sentence case — NOT uppercase Fraunces)
  finished by a short `::after` rule (46×1px terracotta at 50% opacity) +
  the big tight `h1` (see Type above) + `subtitle`/dek (Spectral upright, 21px,
  `--ink-soft`, `max-width: 30ch`, `margin-top: 18px`).
- **Sections** — `h2` in Fraunces; body paragraphs in Spectral. Give each
  `<section>` an `id` so the nav drawer's TOC can link to it.
- **Sources box (`.fuentes`)** — `border-left: 4px solid var(--terracotta)`,
  `background: rgba(176,78,41,0.06)`, links in terracotta with a faint underline.
- **Footer** — italic, `--muted`, small, usually centered, ending with the brand
  line: `Café con Claude · «Serie» · Mes Año`.
- **Reveal-on-scroll** — elements with `.revela` fade/translate in via
  `IntersectionObserver`; always pair with a `prefers-reduced-motion` fallback
  that shows everything immediately.

### Tone of the brand line

Footers read like a colophon, not a UI. Examples pulled from real pages:

- `Café con Claude · Serie «La Red es la Iglesia» · Julio 2026`
- `Café con Claude · Cuaderno de estudio · «Dios me ha hecho reír» — Gn 21:6`
- `Café con Claude · El Panel Pregunta, respuesta nº 4 · Julio 2026`

Pages are predominantly in **Spanish** (`<html lang="es">`); a few are English.
Match the language of the content the user gives you.

---

## Index register (only for `index.html`)

The hub page is deliberately plainer — a hand-edited table of contents:

- Body font `Georgia, "Times New Roman", serif`; sans-serif (`ui-sans-serif,
  system-ui`) only for the small uppercase section labels and tags.
- Palette: `--ink #2C2C2A`, `--paper #FAFAF7`, `--accent #BA7517`, `--muted
  #9A9A93`, `--rule #E7E4DC`; dark header `#1A1A18` with a gold subtitle `#C9A36B`.
- `max-width: 640px`. Sections are collapsible **persianas**: each is a
  `<details class="persiana">` whose `<summary>` holds a chevron (`›`), the
  uppercase section title (`.label-txt`), and an item `.count`, followed by a
  `<ul>` of `<a>` links (optionally a `.tag` span like `Essay`). Sections are
  collapsed by default; a top-right `#toggle-all` button expands/collapses all.
- Footer: `Café con Claude · índice curado a mano, ordenado con Fable`.

- Above the persianas sits a **theme-filter chip row** (`.filtros`): multi-select
  chips whose `data-filtro` values match the `data-tags` on each `<li>`. Active
  chips show the union of matching links; sections auto-open and their counts
  become «n de total»; «Todo» clears the filter. The controlled tag vocabulary is:
  `marcos` · `biblia` · `discipulado` · `metodo` · `sistemas` · `humor` ·
  `historia` · `panel` · `infografia`.

When you add a page to the index (always do this — see SKILL.md), append an
`<li data-tags="…"><a href="…">Title</a></li>` to the `<ul>` inside the most
fitting `<details class="persiana">`, **increment that section's `.count`** by
one, and give the `data-tags` attribute 1–4 themes from the controlled
vocabulary above (a page can — and often should — carry several; that's how it
shows up under more than one filter). Don't invent new tag values without
also adding a chip for them. Don't restyle the index or reorder existing
entries.
