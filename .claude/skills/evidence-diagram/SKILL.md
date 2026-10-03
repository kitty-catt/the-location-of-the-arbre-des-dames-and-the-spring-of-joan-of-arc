---
name: evidence-diagram
description: Build or rebuild a first-hand-only Mermaid object-relationship diagram from prompt/tree-prompt.yaml, following the strict evidentiary rules used for breakout/combined.md — excluding condemnation-trial judges' assertions, drawing thick/double edges for relations Jeanne D'Arc herself stated, and coloring a node green whenever Jeanne D'Arc herself said something that touches it (default styling otherwise). Use when new witness snippets are added, or when asked to diagram relationships between tree/spring/road/land entities.
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
2. **Exclude the condemnation-trial judges' own assertions.** A
   `first-hand-snippet` still doesn't count as evidence if its content is the
   judges'/promoter's own accusatory framing rather than a statement by a
   witness or by Jeanne herself — the judges were never eyewitnesses to
   anything in Domremy. Signals of a judges'-authored snippet: third-person
   accusatory prose with no named witness and no `from` field, especially
   text in the style of the Articles of Accusation (e.g. opening "Item
   ladite Jeanne...", or a trial-summary paragraph asserting things about
   Jeanne without quoting her or a witness). This is different from an
   interrogation transcript where Jeanne's own quoted answer (`R. ...`, or an
   exchange ending in `Jeanne D'Arc - ...`) is what actually supplies the
   relationship — keep those, and attribute them to Jeanne, not to the
   judge/interrogator who asked the leading question. When a snippet's
   authorship is genuinely ambiguous between the two, don't silently decide
   — flag it under judgment calls (rule 7).
3. **No inferred edges.** Draw a relationship between two entities only if a
   single snippet's text *directly* states it. Proximity, distance math, or
   plausibility from `personal-musings` is not a basis for an edge.
4. **Honor the literal name in each snippet, even across synonyms.** When two
   entities are established as the same underlying thing under different
   names (e.g. a snippet states the tree is called both "l'arbre des Dames"
   and "l'arbre des Fées"), attach each new edge to whichever name that
   specific supporting snippet actually uses — do not default an edge to one
   synonym's node just because other edges already point there. Only fold a
   bare/unqualified reference (e.g. plain "l'arbre", "cet arbre") into the
   most recently-named entity in that testimony; an explicit alternate name
   given in the snippet must be honored as its own node's edge, even if a
   "primary" synonym node already has an edge for the same relationship.
5. **Fixed vocabulary, and drop isolated nodes.** Only draw a node for an
   `object-model` entry that (a) is directly named in at least one first-hand
   snippet that survives rule 2, AND (b) has at least one first-hand-stated
   relationship to another included entity. An entry that is named
   first-hand but never appears in a directly-stated relationship must be
   dropped from the diagram entirely — do not draw it as an unconnected
   node. Every other `object-model` entry must be listed as excluded, with
   the reason: never named first-hand, named only in a judges'-authored
   snippet excluded under rule 2, or named first-hand but with no stated
   relationship to any other included entity.
6. **Cite everything.** Every edge gets a numbered relation below the diagram,
   with the exact quote (verbatim, original French — do not translate), its
   `document-reference`, and the witness name if the snippet gives one (`from`
   field or a name embedded in the snippet text itself).
7. **Flag judgment calls.** Where categorizing a snippet is ambiguous (e.g.
   whether two different phrasings of "une fontaine" refer to the same
   spring, whether an unqualified "cet arbre" refers back to a
   previously-named tree in the same testimony, or whether a snippet is a
   judges' assertion under rule 2), state the call and the textual basis
   for it explicitly in its own section — don't resolve it silently.
8. **Double/thick edges for anything Jeanne D'Arc herself said.** If any
   relation supporting an edge comes from a snippet where Jeanne is the
   speaker — `from: Jeanne D'Arc`, or her own quoted answer per rule 2 —
   render that whole edge as a thick/double-weight Mermaid arrow
   (`A ==>|label| B`) instead of the normal-weight arrow (`A -->|label| B`).
   An edge only needs one Jeanne-sourced relation among its supporting
   numbers to qualify; note in the relations list which numbers are
   Jeanne-sourced so the diagram's line weights stay traceable.
9. **Green nodes for anything Jeanne D'Arc herself said something about.**
   No archaeological or fontaine-fiévreux-specific coloring rule anymore —
   every included node gets the diagram's default styling *unless* it is an
   endpoint of at least one thick edge (rule 8), in which case color it
   green (via a Mermaid `classDef`/`class` pair). This is purely derived
   from which edges are already thick — a node qualifies the moment any one
   of its edges does, with no separate citation needed beyond what already
   justifies that edge's thickness.

## Steps

1. Read `prompt/tree-prompt.yaml` in full — specifically `object-model` and
   `first-hand-snippets`.
2. If the target output file exists, read it too, so entity numbering (`N1`,
   `N2`, ...) and relation numbering stay stable for anything unchanged.
3. Filter `first-hand-snippets` per rule 2: drop any snippet that is the
   judges'/promoter's own accusatory assertion rather than a witness's or
   Jeanne's own statement. Keep a short list of what was dropped and why —
   it feeds the new exclusion group in step 8.
4. For each `object-model` entry, check whether it is named in at least one
   snippet that survived step 3. Build the include/exclude lists per rule 5.
5. For each surviving first-hand snippet, extract every directly-stated
   relationship between included entities, attaching each edge to whichever
   entity name the snippet literally uses (rule 4). Number these relations in
   snippet order, and mark which relation numbers are Jeanne-D'Arc-sourced
   (rule 8: `from: Jeanne D'Arc`, or her own quoted answer within an
   interrogation-transcript snippet).
6. Emit the diagram as a Mermaid `graph TD` block:
   - Edges: label with comma-separated relation numbers when more than one
     snippet supports the same edge (existing convention — don't invent a new
     label scheme). Render an edge as thick (`A ==>|1,2| B`) if any of its
     supporting relation numbers is Jeanne-sourced; otherwise normal-weight
     (`A -->|3| B`) (rule 8).
   - Nodes: add one `classDef` (e.g. `classDef jeanneSourced
     fill:#2ecc71,stroke:#1e8449,color:#000;`) and a single `class` line
     assigning it to every node that is an endpoint of at least one thick
     edge (rule 9); leave every other node with the diagram's default
     styling.
7. Below the diagram, write the numbered relations list: each entry gives the
   relation in bold, the verbatim quote in italics, `(document-reference,
   witness)`, and — when it's one of the relations marked in step 5 — a
   "(Jeanne D'Arc)" tag so the thick edges stay traceable to their source.
8. Write the "Included entities and why" and "Excluded entirely" sections,
   plus a "judgment calls" section per rule 7. Split "Excluded entirely" into
   three groups: entries never named in a first-hand snippet, entries named
   only in a snippet dropped under rule 2 (judges' assertion), and entries
   named first-hand but dropped for having no stated relationship (isolated).
9. Write a "Jeanne-sourced coloring" section: one line per green node naming
   the thick-edge relation number(s) that qualify it under rule 9 — no
   separate rationale needed beyond pointing at the edge, since the color
   is purely derived from edge thickness.
10. Save to the target file. If this is an update to an existing file, note
    in your summary to the user exactly what changed (new nodes, new edges,
    newly-thick or newly-green elements, or nothing).
11. Copy the Mermaid diagram block emitted in step 6 — the fenced ` ```mermaid
    ... ``` ` block only, no relations list, no prose, no `classDef`
    explanation — into `README.md`, under the `# AI generated object
    relations based on first hand witness accounts` heading. If a diagram
    block already sits there from a prior run, replace it in place so it
    stays in sync with the latest version; keep the existing link to the
    output file (e.g. `breakout/combined.md`) in that section rather than
    removing it.
12. If asked to reflect this new/updated diagram in `README.md` beyond the
    diagram copy in step 11 — e.g. documenting the prompt that produced it —
    follow the existing convention there: append the exact prompt you were
    given plus a link to the output file, under the relevant section — do
    not overwrite prior entries.

## Output

A single Markdown file at the target path, matching the structure of
`breakout/combined.md`, with thick (`==>`) edges for Jeanne-D'Arc-sourced
relations, normal (`-->`) edges otherwise, and a single green `classDef`
applied to every node touched by at least one thick edge — no other
coloring rule. Do not fabricate entities or relations not present in
`prompt/tree-prompt.yaml` — when in doubt, exclude and say why.
Additionally, `README.md`'s "AI generated object relations based on first
hand witness accounts" section always carries a synced copy of that same
diagram block (diagram only) alongside its existing link to the output file.
