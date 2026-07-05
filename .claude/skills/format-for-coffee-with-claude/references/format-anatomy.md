# Café con Claude format — the elements a formatted file must contain

This is the target shape for any file run through this skill, taken from the
site's study pieces (`el_caracter_de_jesus.html`, `vestirse-para-ser-amado.html`,
`el-mono-y-el-ave-v3.html`). The job is to **reformat existing content** into this
anatomy — reorganize, label, and add the house scaffolding — while keeping the
author's substance and voice. You are not researching or inventing; you are
tailoring.

The YAML front-matter fields are named to line up 1:1 with the `cafe-con-claude`
HTML skill, so a formatted file can be rendered straight into a page.

Labels come in two languages; use the set that matches the document's language.

| Element        | Spanish label        | English label |
|----------------|----------------------|---------------|
| itinerary/TOC  | `El recorrido`       | `The itinerary` |
| key takeaway   | `Idea clave.`        | `Key idea.` |
| synthesis      | `Síntesis`           | `Synthesis` |
| close          | `Cierre`             | `Close` |
| bibliography   | `Fuentes consultadas`| `Sources` |
| chips label    | `Fuentes de un vistazo` | `At a glance` |
| thesis lead    | `En una línea:`      | `In one line:` |

---

## The cover (everything above the first section)

1. **Eyebrow / scope line** — a short overline naming the genre and scope. Derive
   it from what the content *is* (an essay, a study, a guide) and what it covers.
   Dot-separate the scope. → front-matter `eyebrow:`.
2. **Title** + optional **italic second line** — use the input's title if it has a
   strong one; otherwise write one from the content. The second line is a poetic
   or clarifying complement. → `title:` and `subtitle:`; rendered as `# Title` then
   `*Second line*`.
3. **Standfirst (dek)** — 2–3 sentences summarising the whole piece: the subject,
   the angle, the stakes. Write it from the content. → an italic blockquote under
   the title.
4. **One-line thesis** — the core claim compressed to one sentence. → a bold
   `**En una línea:** …` line.
5. **Status / edition** — a small living-document stamp ("Documento vivo · edición
   1"). → `status:` in front-matter, echoed as a small line.
6. **Chips (at a glance)** — the handful of primary sources or the scope, surfaced
   as tags. **Only from the input**: if the source names references, list them; if
   it doesn't, use scope tags derived from the content, or drop the line. Never
   invent citations. → `sources_at_a_glance:` in front-matter + an inline
   `` `chip` · `chip` `` line.
7. **Divider** — `---` closing the cover.

## The itinerary

8. **`## El recorrido`** — a table of contents. Build it from the document's real
   sections (or from the parts you chunk the content into). Number ordinary parts
   (I, II, III…) and label the special ones (Síntesis, Cierre, Apéndice · Fuentes).
   Link each entry to its anchor.

## The body

9. **Numbered parts** — reflow the content into numbered parts, one per idea/section
   in the source. Open each with a small part label, then `## II · Heading`, then
   the author's prose (lightly edited for flow, not rewritten).
10. **"Idea clave" callout** — end every part with its single takeaway, extracted
    from that part's content, so a skimmer can harvest the argument from the callouts
    alone. → `**Idea clave.** …`. This is the signature move; include it on every part.
11. **Pull quotes** — lift one memorable line per part or two into a blockquote
    `> …`, drawn from the existing text.
12. **Per-section sources** — if the input attributes claims in a section, carry
    those into `*Fuentes de la sección:* [Author, Title](url) · …`. Omit where the
    source doesn't provide them.

## The close

13. **`## Síntesis · …`** — the piece's overall point, pulled together. Use the
    input's conclusion if it has one; otherwise synthesise from the parts.
14. **`## Cierre · …`** — what the reader does next: applications, questions, next
    steps. Use the input's ending if present, else write a short one.
15. **`## Fuentes consultadas`** — collect every source the input cites, with a
    one-line note each. **If the input cites none, say so** with a clearly-marked
    placeholder (`> _Sin fuentes en el original — pendiente de añadir._`) rather
    than inventing any.
16. **Colophon** — a final small line recording the model + effort that formatted
    the file (read from your session context).

---

## Fidelity rules (this is a formatter, not an author)

- **Keep the author's content and claims.** Reorganize, label, and trim for flow;
  don't add new facts, arguments, or sources.
- **Never fabricate citations, quotes, or data.** Missing sources get a visible
  placeholder, not invention.
- **Match the document's language** for prose and for the labels in the table above.
- **Preserve emphasis and terminology** the author chose.

## Mapping to the `cafe-con-claude` page skill

| This file              | `cafe-con-claude` |
|------------------------|-------------------|
| `eyebrow:`             | `{{EYEBROW}}` / drawer `{{SERIES}}` |
| `title:` / `subtitle:` | `{{H1}}` (+ italic line) |
| standfirst blockquote  | `{{SUBTITLE}}` |
| `## El recorrido`      | drawer `{{TOC}}` |
| numbered parts         | `{{BODY}}` sections (`id="s1"`…) |
| `## Fuentes consultadas` | `.fuentes` sources box |
| colophon               | `.model-note` |
