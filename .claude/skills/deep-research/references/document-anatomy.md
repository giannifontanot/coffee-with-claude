# Deep-research document anatomy

The output is a Markdown research report shaped like the Café con Claude study
pieces (`el_caracter_de_jesus.html`, `vestirse-para-ser-amado.html`,
`el-mono-y-el-ave-v3.html`). Those pages have a very deliberate structure; this
file is the map. Each element below lists what it is, why it earns its place, and
how to render it in Markdown. The YAML front-matter fields are named to line up
1:1 with the `cafe-con-claude` HTML skill, so a finished report can be handed
straight to that skill to become a page.

Reproduce the *elements*, not the exact wording — the examples are Spanish study
notes; your report adapts the pattern to the actual topic.

---

## The cover (everything above the first section)

1. **Eyebrow / scope line** — a short overline naming the genre and scope of the
   piece. It orients the reader in one glance. Dot-separate the scope.
   *Examples:* `UN RECORRIDO POR LOS CUATRO EVANGELIOS` · `ESTUDIO BÍBLICO ·
   MARCOS 10 · 1 Y 2 SAMUEL`. → front-matter `eyebrow:`.
2. **Title** + optional **italic second line** — a plain main title and a poetic
   or clarifying second line. → front-matter `title:` and `subtitle:`; rendered as
   `# Title` then `*Second line*`.
3. **Standfirst (the dek)** — 2–3 sentences that summarise the whole piece: the
   question, the angle, and the stakes. This is the abstract. → an italic
   blockquote right under the title.
4. **One-line thesis** — the crux compressed to a single sentence ("Todo gira
   sobre un solo error y su remedio…"). → a bold `**En una línea:** …` line.
5. **Status / edition** — a small meta stamp signalling this is a living document
   ("Documento vivo · edición ampliada"). → `status:` in front-matter, echoed as a
   small line. Bump the edition when you revise.
6. **Sources at a glance (chips)** — the handful of primary sources/scope surfaced
   up front as tags (`Mateo · Marcos · Lucas · Juan`; `Marcos 10:17–22`). This
   tells the reader what the research rests on before they commit. → `sources_at_a_glance:`
   list in front-matter, echoed as an inline `` `chip` · `chip` `` line.
7. **Divider** — a rule (`---`) closing the cover. (On the rendered page this is
   the engraved-leaf motif.)

## The itinerary

8. **`## El recorrido`** — a table of contents / overview of the parts. Number the
   ordinary parts (I, II, III…) and give the special ones their role labels:
   **Síntesis**, **Cierre**, **Apéndice · Fuentes**. Link each entry to its section
   anchor. This doubles as the reading plan.

## The body

9. **Numbered parts** — each substantive section is a numbered part. Open it with a
   small **part label** (roman numeral or a word like *Apertura*, *Primera parte*),
   then the `## II · Heading`, then the prose.
10. **"La idea clave" callout** — every part ends with its single key takeaway, set
    off so a skimmer can harvest the argument from the callouts alone. →
    `**Idea clave.** …`. This is the signature move of these documents — do not skip
    it.
11. **Pull quotes** — one memorable line per part or two, lifted out. → a Markdown
    blockquote `> …`.
12. **Per-section sources** — the parts that lean on specific evidence name it at
    the end of the section, so claims are traceable where they're made. →
    `*Fuentes de la sección:* [Author, Title](url) · [Author, Title](url)`.

## The close

13. **`## Síntesis · …`** — a synthesis section that pulls the parts into one
    picture ("El retrato y el espejo"). Not a summary of what was said — the
    *answer* the research arrived at.
14. **`## Cierre · …`** — a closing that hands the reader something to do or carry
    ("Para tu clase"): applications, open questions, next steps.
15. **`## Fuentes consultadas`** (Apéndice) — the full bibliography: every source,
    with a one-line note on what each supports. Deep research lives or dies on this.
16. **Colophon** — a final small line recording the model and effort that produced
    the report, mirroring the page skill's model-note (`*Investigación generada por
    Claude Opus 4.8 · esfuerzo alto · 2026-07-05.*`).

---

## How this maps to the `cafe-con-claude` page skill

If the report will be rendered as a page, the mapping is direct:

| This report            | `cafe-con-claude` |
|------------------------|-------------------|
| `eyebrow:`             | `{{EYEBROW}}` / drawer `{{SERIES}}` |
| `title:` / `subtitle:` | `{{H1}}` (+ italic line) |
| standfirst blockquote  | `{{SUBTITLE}}` |
| `## El recorrido`      | drawer `{{TOC}}` |
| numbered parts         | `{{BODY}}` sections (`id="s1"`…) |
| `## Fuentes consultadas` | `.fuentes` sources box |
| colophon               | `.model-note` |

So the natural pipeline is **deep-research → `.md` → (optional) cafe-con-claude → page.**
