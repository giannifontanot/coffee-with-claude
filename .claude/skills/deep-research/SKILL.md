---
name: deep-research
description: >-
  Do a deep research on a topic and write the findings to a Markdown (.md) file.
  Use this whenever the user says "do a deep research", "deep research", "research
  this deeply", "investiga a fondo", or otherwise asks for a thorough, cited
  written investigation of a subject (a person, question, event, concept, claim,
  or body of work) delivered as a document. The report follows the Café con Claude
  study-piece anatomy — eyebrow/scope, title, standfirst, one-line thesis,
  "fuentes de un vistazo" chips, an "El recorrido" table of contents, numbered
  parts each ending in a "Idea clave" takeaway, a Síntesis, a Cierre, and a full
  "Fuentes consultadas" bibliography — and its front-matter maps straight onto the
  cafe-con-claude page skill so the .md can later be rendered as a page.
---

# Deep research → Markdown report

This skill turns a topic into a genuinely researched, well-cited Markdown
document shaped like the site's study pieces. Two references do the heavy lifting;
read them before you start:

- `references/document-anatomy.md` — every element the report must contain, why it
  earns its place, and how to render it in Markdown. Read this first.
- `assets/template.md` — the fill-in scaffold. Copy it and replace the `{{…}}`.

The goal is *research*, not a nicely formatted opinion. The structure is only
worth anything if it's carrying real findings with real sources behind them.

## Workflow

### 1. Frame the question

Restate the topic in one sentence so you and the user agree on it. Then decompose
it into **4–8 sub-questions** — the things you'd actually have to answer to speak
with authority. These become candidate sections. Decide:

- **Language** — match the topic/request; default to Spanish for study pieces.
- **Scope** — what's in and what's out (this becomes the eyebrow + the chips).
- **Angle** — deep research still needs a spine. What's the question *behind* the
  question? (In the examples: not "what does Jesus teach" but "what is Jesus
  *like* — the verbs that repeat.")

### 2. Research (this is the part that makes it "deep")

Use whatever web tools you have — `WebSearch` to find sources, `WebFetch` to read
them. For each sub-question:

- Run **several** searches with different phrasings; don't stop at the first hit.
- Open and actually read the **primary** or most authoritative sources, not just
  search snippets. Aim to triangulate each substantive claim across **more than
  one** source.
- Capture, as you go, for every source you'll cite: author/title, publication, the
  URL, and one line on what it supports. You'll need these for the per-section and
  final bibliographies.
- Note **disagreements and uncertainty** explicitly — where sources conflict, or
  where the evidence is thin. A deep-research doc that only reports consensus is
  hiding the interesting part.

Some hosts block automated fetching (e.g. The Guardian returns 403). When a source
won't load, say so and lean on other sources rather than fabricating its contents.
If you have **no** web access at all, tell the user the report will be built from
existing knowledge with source slots to fill, and proceed only if they're fine
with that — never invent URLs or citations to look researched.

### 3. Structure the findings

Copy `assets/template.md` to the output file and fill the anatomy. Order of
writing that works well:

1. Draft the **numbered parts** first — one per sub-question that survived the
   research — each ending in its **"Idea clave"** takeaway and its
   *Fuentes de la sección*. The Idea clave lines are the backbone; if you can read
   them top to bottom and get the argument, the structure is right.
2. Write the **Síntesis** (the answer the research reached, tensions included) and
   the **Cierre** (what the reader does next).
3. Only now write the **standfirst** and the **one-line thesis** — they're easy
   and honest once you know what you actually found.
4. Fill the **cover** (eyebrow, chips, status/edition, date) and assemble
   **`## El recorrido`** and **`## Fuentes consultadas`**.

### 4. Write the file and sign it

Save as a kebab-case `.md` (e.g. `research/la-historia-del-espresso.md`; create a
`research/` folder if the repo doesn't have one, or write to the working directory
if you're not in this repo). End with the **colophon** naming the model and effort
that produced it — read the model from your session context, same as the page
skill's model-note.

### 5. Verify (checklist)

- [ ] Front-matter has `title`, `eyebrow`, `lang`, `status`, `date`, and
      `sources_at_a_glance`.
- [ ] Standfirst, one-line thesis, and the chips line are all present.
- [ ] `## El recorrido` lists every part and the Síntesis / Cierre / Fuentes.
- [ ] Every numbered part ends in a **`Idea clave.`** line.
- [ ] Every substantive claim is traceable — per-section sources where it's made,
      and a full **`## Fuentes consultadas`** with real, distinct URLs.
- [ ] Síntesis states the answer (and any disagreement/uncertainty found), not a
      recap; Cierre gives the reader next steps.
- [ ] Colophon names the model + effort.
- [ ] No `{{…}}` placeholders remain.

Tell the user the path to the file and give a 2–3 line summary of what the
research concluded.

## Turning the report into a page (optional)

The front-matter is named to feed the `cafe-con-claude` skill directly — see the
mapping table at the end of `references/document-anatomy.md`. If the user wants the
research as a Café con Claude page, hand the finished `.md` to that skill: eyebrow
→ `{{EYEBROW}}`, title/subtitle → `{{H1}}`, standfirst → `{{SUBTITLE}}`,
`## El recorrido` → the drawer TOC, parts → `{{BODY}}`, `## Fuentes consultadas` →
the sources box, colophon → the model-note.

## Files

- `references/document-anatomy.md` — the full element inventory + Markdown mapping.
- `assets/template.md` — the fill-in-the-blanks report scaffold.
