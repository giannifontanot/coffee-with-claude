---
name: cafe-con-claude
description: >-
  Generate a new "Café con Claude" / "Coffee with Claude" HTML reading-room page
  that matches the site's existing house style (Fraunces + Spectral serif type on
  a warm cream/terracotta palette). Use this whenever the user asks to create,
  draft, write, or lay out a new page, article, essay, study note, infographic, or
  "panel" piece for Café con Claude / Coffee with Claude, or to turn a piece of
  writing into a page for this site — even if they don't name the design system.
  Every page it produces has a top back control that runs history.back() and a
  small colophon at the bottom stating which model and effort produced it.
---

# Café con Claude page generator

This skill builds a single self-contained HTML page in the visual language of the
Café con Claude reading room. The design system was reverse-engineered from the
real pages in this repo; the specifics live in `references/design-system.md` and a
ready-to-fill skeleton lives in `assets/template.html`.

Read `references/design-system.md` before writing CSS so your page inherits the
real palette, fonts, and component conventions instead of inventing new ones. The
whole point of the site is visual consistency — a page that looks "close but not
quite" is worse than one that reuses the exact tokens.

## The things that must always be true

These are the reasons this skill exists, so don't let them slip:

1. **A top-left hamburger opens the nav drawer.** The site's pages carry a fixed
   hamburger (`#nav-toggle`, `☰`) in the top-left corner that slides out an
   off-canvas drawer holding the brand and a table of contents of the page's
   sections (see `porque-tenian-miedo.html`, `dichas-una-sola-vez.html`). It is
   the standard way readers jump around a long piece, so include it. The template
   already wires the toggle, the scrim, and a scroll-spy that highlights the
   current section — your job is to fill the TOC (`{{TOC}}`) with one `<li>` per
   `<section>`, and to give each `<section>` a matching `id`. For a genuinely
   short page (one or two sections) a TOC adds nothing; it's fine to drop the
   drawer then, but that's the exception, not the default.

2. **The back control runs browser history, not a hard link.** The reader arrives
   at a page from many places — the index, another article, a search result, a
   shared link. A fixed `<a href="index.html">` would yank them somewhere they
   didn't come from. So the control is a `<button>` calling `history.back()`,
   which returns them to wherever they actually were, with a graceful fallback to
   `index.html` *only* when `history.length <= 1` (a cold-opened tab with nothing
   to go back to). Keep it a `<button>`; don't "simplify" it into a plain link.

3. **A small colophon at the bottom names the model and effort.** These pages are
   made by a model, and the author wants that on the record. Right under the
   footer brand line, emit a `<p class="model-note">` stating the model powering
   *this* session and the reasoning effort in use — see "The model note" below.

## Workflow

1. **Gather the content.** You need: the title, the language (default `es` — most
   pages are Spanish; use `en` if the content is English), an optional eyebrow /
   kicker (e.g. `El Panel Pregunta · Nº 3` or a series name), a one-line italic
   subtitle, the body, optional sources, and the footer brand line. If the user
   handed you raw text, shape it into sections yourself; ask only if something
   essential is genuinely missing.

2. **Read the design reference.** Open `references/design-system.md`. Confirm the
   palette and the Fraunces + Spectral font load. Decide width: `820px` for
   long-form/panel pieces, `640px` for tight essays.

3. **Start from the template.** Copy `assets/template.html` to the new file
   (kebab-case name ending in `.html`, e.g. `el-nuevo-articulo.html`, in the repo
   root next to the other pages). Replace every `{{PLACEHOLDER}}`:

   | Placeholder      | Fill with |
   |------------------|-----------|
   | `{{LANG}}`       | `es` or `en` |
   | `{{TITLE}}`      | Page title (the ` · Café con Claude` suffix is already in `<title>`) |
   | `{{BRAND}}`      | Drawer brand mark: `Café con Claude` (plain — no emoji) |
   | `{{SERIES}}`     | Drawer sub-line: the series/section, e.g. `América a los 250` |
   | `{{TOC}}`        | One `<li>` per section, e.g. `<li><a href="#s1"><span class="num">I</span>Section title</a></li>` — the `href` must match each `<section>`'s `id` |
   | `{{BACK_LABEL}}` | Back-control text, e.g. `Volver al índice`, `Sala de lectura`, `Back to the index` |
   | `{{EYEBROW}}`    | Kicker/series label, or delete the `<span class="eyebrow">` line |
   | `{{H1}}`         | Headline |
   | `{{SUBTITLE}}`   | One-line italic subtitle |
   | `{{BODY}}`       | The article: `<section id="s1">` blocks with `<h2>` + `<p>`. Give each section an `id` that a TOC entry points to. Add `class="revela"` to sections you want to fade in on scroll |
   | `{{SOURCES}}`    | Source links, or delete the whole `.fuentes` block if none |
   | `{{FOOTER_LINE}}`| Brand colophon, e.g. `Café con Claude · Serie «La Red es la Iglesia» · Julio 2026` |
   | `{{MODEL_NOTE}}` | See below |

   Keep the TOC and the section ids in sync — the hamburger menu and the
   scroll-spy read the `#id` in each TOC link. Delete any optional block you
   don't use (progress bar, sources box, reveal-on-scroll) — but never delete the
   hamburger drawer (except on a one/two-section page), the back button, or the
   model note.

4. **Build the body in-style.** Match the surrounding pages: `<h2>` in Fraunces,
   prose in Spectral, accents via the CSS variables. For richer pieces you can add
   the recurring components described in the reference (sticky TOC sidebar, source
   box, timeline/"spine"). Don't reach for new fonts or colors unless asked.

5. **Verify before finishing** (see checklist).

6. **Register it in the index (always do this).** Linking the new page from
   `index.html` is a standard part of the job, not an optional extra — the index
   is the site's front door, and a page that isn't listed there is effectively
   lost. So don't ask whether to do it; do it. Read the current `index.html`,
   pick the section whose theme best fits the new page (match by topic — a Marcos
   study goes under the Marcos section, a discipleship piece under «Discipulado y
   carácter», an infographic under «Infografías», etc.), and append one
   `<li data-tags="…"><a href="new-file.html">Title</a></li>` — optionally with a
   `<span class="tag">…</span>` label — to the end of that section's list. Fill
   `data-tags` with 1–4 themes from the index's controlled vocabulary (see the
   index register in `references/design-system.md`) so the page appears under
   the right filter chips; also bump the section's `.count`. If no
   section genuinely fits, add a new one in the index's existing style. Don't
   restyle the index or reorder the entries that are already there. Note the
   index's sections may be plain `<section>`s or collapsible `<details>`
   accordions; insert into whichever container holds that section's list.

7. **Mark the source Markdown as converted.** When the page was rendered from a
   source `.md` file that lives in the repo (e.g. `research/mi-tema.md`), rename
   that file once the HTML exists and is registered — insert `.publicado` before
   the extension: `research/mi-tema.md` → `research/mi-tema.publicado.md` (use
   `git mv` so history follows). This makes it obvious at a glance which research
   docs are already on the site: the pending ones are those *without*
   `.publicado.` in the name. Only rename a real source file you actually
   consumed — never invent one, and skip this step when the content came from
   pasted text rather than a repo file. As a durable record, also add
   `rendered_to: mi-tema.html` and `rendered_on: YYYY-MM-DD` to the MD's YAML
   front-matter if it has one.

## The model note

Fill `{{MODEL_NOTE}}` with the model that is generating the page right now and the
reasoning effort it is running at. Read the model identity and effort from your
own session context (the system prompt names the model; the effort is your current
thinking/reasoning level). Write it as a quiet colophon, matched to the page
language:

- Spanish: `Generada por Claude Opus 4.8 · esfuerzo alto`
- English: `Made with Claude Opus 4.8 · high effort`

Use the real display name of the current model (e.g. `Claude Opus 4.8`,
`Claude Sonnet 5`, `Claude Fable 5`), not a guess. If you genuinely can't
determine the effort level, name the model and drop the effort clause rather than
inventing one — an accurate half is better than a confident fabrication. Keep it
to one short line; it's a footnote, not a banner.

## Verification checklist

Before you call it done, open the file and confirm:

- [ ] No `{{PLACEHOLDER}}` tokens remain.
- [ ] The top-left `#nav-toggle` hamburger exists and the drawer's `{{TOC}}` has
      one entry per section, each `href="#id"` matching a real `<section id>`.
- [ ] The back control is `<button class="back-link" ... onclick="...history.back()...">`
      — a button, not an `<a href>`.
- [ ] A `<p class="model-note">` exists near the bottom with a real model name.
- [ ] Fonts are Fraunces + Spectral; the `:root` palette matches the reference.
- [ ] `<html lang>` matches the content language.
- [ ] The page is linked from `index.html` (in the fitting section, count bumped).
- [ ] If rendered from a repo `.md` source, that file is renamed to `*.publicado.md`.
- [ ] It's a valid, self-contained page (no unclosed tags; opens standalone).

If you have a browser tool available, load the page and: click the hamburger to
confirm the drawer opens and a TOC link scrolls to its section, and click the
back control to confirm it navigates through history rather than to a fixed URL.

## Files

- `references/design-system.md` — the extracted palette, fonts, layout, and
  component conventions (both the article register and the index register).
- `assets/template.html` — the fill-in-the-blanks page skeleton, already wired
  with the back button and the model-note colophon.
