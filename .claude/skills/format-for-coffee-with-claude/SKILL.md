---
name: format-for-coffee-with-claude
description: >-
  Reformat an existing file or draft into the Café con Claude / Coffee with Claude
  Markdown format, guaranteeing the house elements are present. Use this whenever
  the user says "format for coffee-with-claude", "format this for coffee with
  claude", "cafe con claude format", "put this in the coffee-with-claude format",
  or hands you an article, essay, notes, transcript, or draft and asks to shape it
  into the site's study-piece layout. It restructures the content you give it —
  it does not research or add facts — and always produces: eyebrow/scope, title
  (+ italic line), standfirst, one-line thesis, status/edition, "fuentes de un
  vistazo" chips, an "El recorrido" table of contents, numbered parts each ending
  in a "Idea clave" takeaway, a Síntesis, a Cierre, and a "Fuentes consultadas"
  section. Its front-matter maps onto the cafe-con-claude page skill.
---

# Format for Café con Claude

This skill takes a file (or pasted text) and reshapes it into the Café con Claude
study-piece format as a Markdown document, ensuring every house element is
present. It is a **formatter, not an author**: it reorganizes and labels the
content you give it and writes the connective scaffolding (eyebrow, standfirst,
"Idea clave" callouts, itinerary), but it does not research, invent facts, or add
citations that weren't in the source.

Read these before you start:

- `references/format-anatomy.md` — the target elements, the two-language label set,
  the fidelity rules, and how the output maps onto the `cafe-con-claude` page skill.
- `assets/template.md` — the fill-in scaffold. Copy it and replace the `{{…}}`.

## Workflow

### 1. Read and inventory the input

Read the whole file. Note:

- **Language** (match it in the output — prose *and* labels; see the table in the
  anatomy reference).
- **Existing structure** — headings, sections, natural breaks. These become the
  numbered parts and the itinerary.
- **A title/subject**, if present.
- **Any sources/citations/links** the author already included — you'll carry these
  into the per-section and final bibliographies. If there are none, note that; you
  will *not* add any.
- **A conclusion/call to action**, if present — feeds the Síntesis / Cierre.

### 2. Map content → anatomy

Copy `assets/template.md` to the output file and fill it from the input:

- **Title / italic line** — keep the author's title (sharpen only if weak); add a
  second line if the content suggests one.
- **Eyebrow** — name the genre + scope from what the piece is and covers.
- **Standfirst** — write 2–3 sentences summarising the input.
- **One-line thesis** — the input's core claim in a single sentence.
- **Chips** — references named in the input, or scope tags derived from it; omit if
  neither fits. Never fabricate.
- **`## El recorrido`** — from the input's real sections (or the parts you chunk it
  into).
- **Numbered parts** — reflow the author's prose into parts. Keep their words;
  reorganize and lightly edit for flow. End each with an **`Idea clave.`** you
  extract from that part, and lift one **pull quote** from its text.
- **Síntesis / Cierre** — from the input's conclusion and closing; synthesise only
  from what's there.
- **`## Fuentes consultadas`** — collect the input's sources. If it has none, keep
  the visible `> _Sin fuentes en el original — pendiente de añadir._` placeholder
  and tell the user.
- **Colophon** — model + effort + date, from your session context.

### 3. Hold the line on fidelity

The value of a formatter is that the reader can trust the content is still the
author's. So: don't introduce new claims, examples, or sources; preserve the
author's terminology and emphasis; only cut for redundancy and reorder for the
anatomy. If the input is too thin to fill a section honestly (e.g. no possible
Síntesis), leave a short, clearly-marked note rather than padding it.

### 4. Write the file

Save as a kebab-case `.md` — next to the input if you can infer where it lives,
otherwise in the working directory (or a `research/`-style folder if the user
keeps one). Tell the user the path.

### 5. Verify (checklist)

- [ ] Front-matter has `title`, `eyebrow`, `lang`, `status`, `date` (and
      `sources_at_a_glance` when the input supports it).
- [ ] Standfirst, one-line thesis, and — when applicable — the chips line present.
- [ ] `## El recorrido` lists every part plus Síntesis / Cierre / Fuentes.
- [ ] Every numbered part ends in a **`Idea clave.`** line.
- [ ] Síntesis and Cierre present.
- [ ] `## Fuentes consultadas` lists the input's real sources, or shows the
      "sin fuentes" placeholder — no invented citations.
- [ ] Colophon names the model + effort.
- [ ] Labels are in the document's language; no `{{…}}` placeholders remain.
- [ ] Spot-check: the content is still the author's — nothing new was asserted.

## Turning the formatted file into a page (optional)

The front-matter feeds the `cafe-con-claude` skill directly (see the mapping table
in `references/format-anatomy.md`). If the user wants the formatted file as a page,
hand the `.md` to that skill.

## Files

- `references/format-anatomy.md` — the element inventory, labels, and fidelity rules.
- `assets/template.md` — the fill-in-the-blanks scaffold.
