---
name: evidence-diagram
description: Build or rebuild a first-hand-only Mermaid object-relationship diagram from prompt/tree-prompt.yaml, following the strict evidentiary rules used for breakout/combined.md. Use when new witness snippets are added, or when asked to diagram relationships between tree/spring/road/land entities.
---

# Evidence diagram

Regenerate an object-relationship diagram of the tree, spring, and surrounding
entities named in `prompt/tree-prompt.yaml`, using ONLY first-hand testimony
as evidence. This reproduces the method already used for `breakout/combined.md`.

Arguments (`$ARGUMENTS`, all optional): an output file path (default
`breakout/combined.md`). If the file already exists, treat this as an update —
read it first, keep its structure, and fold in anything new.

## Rules (do not violate these)

1. **First-hand only.** Only entries in the `first-hand-snippets` array of
   `prompt/tree-prompt.yaml` count as evidence. `general-reputation`,
   `observation-list`, `personal-musings`, and `candidate-locations` are
   context/hypothesis — never draw a node or edge from them alone.
2. **No inferred edges.** Draw a relationship between two entities only if a
   single snippet's text *directly* states it. Proximity, distance math, or
   plausibility from `personal-musings` is not a basis for an edge.
3. **Fixed vocabulary.** Only draw nodes for entries that appear in the
   `object-model` array AND are directly named in at least one first-hand
   snippet. Every other `object-model` entry must be listed as excluded, with
   the reason (never named first-hand, or named but with no first-hand
   relation stated).
4. **Cite everything.** Every edge gets a numbered relation below the diagram,
   with the exact quote (verbatim, original French — do not translate), its
   `document-reference`, and the witness name if the snippet gives one (`from`
   field or a name embedded in the snippet text itself).
5. **Flag judgment calls.** Where categorizing a snippet is ambiguous (e.g.
   whether two different phrasings of "une fontaine" refer to the same
   spring, or whether an unqualified "cet arbre" refers back to a
   previously-named tree in the same testimony), state the call and the
   textual basis for it explicitly in its own section — don't resolve it
   silently.

## Steps

1. Read `prompt/tree-prompt.yaml` in full — specifically `object-model` and
   `first-hand-snippets`.
2. If the target output file exists, read it too, so entity numbering (`N1`,
   `N2`, ...) and relation numbering stay stable for anything unchanged.
3. For each `object-model` entry, check whether it is named in at least one
   first-hand snippet. Build the include/exclude lists per rule 3.
4. For each first-hand snippet, extract every directly-stated relationship
   between included entities. Number these relations in snippet order.
5. Emit the diagram as a Mermaid `graph TD` block, edges labeled with
   comma-separated relation numbers when more than one snippet supports the
   same edge (this is the existing convention — follow it, don't invent a new
   label scheme).
6. Below the diagram, write the numbered relations list: each entry gives the
   relation in bold, the verbatim quote in italics, and `(document-reference,
   witness)`.
7. Write the "Included entities and why" and "Excluded entirely" sections,
   plus a "judgment calls" section per rule 5.
8. Save to the target file. If this is an update to an existing file, note in
   your summary to the user exactly what changed (new nodes, new edges, or
   nothing).
9. If asked to reflect this new/updated diagram in `README.md`, follow the
   existing convention there: append the exact prompt you were given plus a
   link to the output file, under the relevant section — do not overwrite
   prior entries.

## Output

A single Markdown file at the target path, matching the structure of
`breakout/combined.md`. Do not fabricate entities or relations not present in
`prompt/tree-prompt.yaml` — when in doubt, exclude and say why.
