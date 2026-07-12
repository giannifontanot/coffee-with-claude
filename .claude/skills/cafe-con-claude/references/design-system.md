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

- **Display / headings — `Fraunces`, serif.** Weights 400/600/700, optical size
  axis. Used for `h1`, `h2`, eyebrows, brand marks. Headline sizing uses
  `clamp()`, e.g. `h1 { font-size: clamp(2rem, 6vw, 3.1rem); line-height: 1.1; }`.
- **Body — `Spectral`, serif.** Weights 400/600 plus italics. `line-height: 1.65`.
- **Eyebrows / labels / mono accents — `DM Mono`, monospace** (optional). Uppercase,
  wide letter-spacing.

Standard `<head>` load:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,600;9..144,700&family=Spectral:ital,wght@0,400;0,600;1,400;1,600&display=swap" rel="stylesheet">
```

`Playfair Display` is an acceptable alternative display face on a few pages, but
prefer Fraunces + Spectral unless the user asks otherwise.

### Palette (the "cream / terracotta" system)

These are the most frequent hex values across the corpus. Put them in `:root`:

```css
:root {
  --cream:     #f7f1e5;  /* page background (paper) */
  --ink:       #2b2118;  /* primary text */
  --terracotta:#b4532a;  /* primary accent: links, rules, back-link */
  --amber:     #c98a2d;  /* secondary accent / highlights */
  --olive:     #6b7a3f;  /* tertiary accent */
  --plum:      #6e4a6e;  /* tertiary accent */
  --line:      #d9cdb8;  /* hairline borders */
  --muted:     #6a5b48;  /* muted / secondary text */
  --card:      #fcf8ef;  /* card & panel backgrounds */
}
```

Optional extras seen in the wild: `--steel/--teal #4a6b7a`, and a near-black
`#1a1a18` for occasional dark hero headers. Accent tints are made with `rgba`,
e.g. `rgba(180,83,42,0.06)` (terracotta wash) for source boxes and hover states.

### Layout

- Centered single column. Article width `max-width: 820px` (long-form) or `640px`
  (tight essays). Wrap in `.wrap { max-width: 820px; margin: 0 auto; }`.
- `body { background: var(--cream); color: var(--ink); font-family: 'Spectral', serif;
  line-height: 1.65; padding: 3rem 1.25rem 5rem; }`
- `html { scroll-behavior: smooth; }`

### Recurring components

- **Hamburger + nav drawer** — a fixed `☰` button in the **top-left** corner
  (`#nav-toggle`, dark `--ink` on `--cream`) toggles an off-canvas `nav.sidebar`
  that slides in from the left over a dim scrim. The drawer holds the brand mark
  (`Café con Claude`, plain — no emoji), a series sub-line, and an `<ol>` table of contents whose
  links (`#s1`, `#s2`, …) point at the page's section ids; a scroll-spy adds
  `.active` to the current one. This is the site's standard navigation for any
  multi-section piece — see `porque-tenian-miedo.html`, `dichas-una-sola-vez.html`.
  The drawer is **responsive**, matching the house convention (see
  `vestirse-para-ser-amado.html`): on desktop (`min-width: 1180px`) it docks as a
  fixed left rail — always visible, no overlay — with the hamburger and scrim
  hidden and the article column shifted right (re-centring past `1500px`); below
  `1180px` it goes off-canvas and the top-left `☰` hamburger slides it in over a
  scrim. Never leave the hamburger collapsed on desktop.
- **Reading progress bar** — fixed, full-width, 3px, gradient of the accents,
  `z-index:100`. Width driven by scroll in JS. Common but optional.
- **Back control (top)** — a link-styled control at the very top. See SKILL.md:
  in this skill it is a `<button>` that runs `history.back()`, NOT an `<a href>`.
  Visual style is the classic `.home-link`: small, terracotta text with a
  terracotta bottom-border, `← ` glyph prefix, `margin-bottom: 2.2rem`.
- **Header** — optional `eyebrow`/`kicker` (uppercase, accent, letter-spaced) +
  `h1` (Fraunces, clamp) + `subtitle` (italic, `--muted`, `max-width: 34em`).
- **Sections** — `h2` in Fraunces; body paragraphs in Spectral. Give each
  `<section>` an `id` so the nav drawer's TOC can link to it.
- **Sources box (`.fuentes`)** — `border-left: 4px solid var(--terracotta)`,
  `background: rgba(180,83,42,0.06)`, links in terracotta with a faint underline.
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
