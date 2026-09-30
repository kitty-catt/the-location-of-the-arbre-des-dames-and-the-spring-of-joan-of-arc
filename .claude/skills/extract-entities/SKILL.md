---
name: extract-entities
description: Extract animate entities (people by role/title, supernatural beings, afflicted groups) named in prompt/tree-prompt.yaml into a structured prompt/entities.yaml, grouped by entity type with each type's array of named/described instances. Use when asked to catalog, extract, or update the entities, characters, or witnesses named in the source material.
---

# Extract entities

Build or update a catalog of animate entities (people, supernatural beings,
afflicted/collective classes) named in `prompt/tree-prompt.yaml`, grouped by
entity type — e.g. type `chevalier` has instances `Pierre de Bourlemont` and
`Albert d'Ourches`.

Arguments (`$ARGUMENTS`, optional): an output path (default
`prompt/entities.yaml`). If it already exists, treat this as an update — read
it first and fold in anything new rather than starting over; keep existing
types/instances untouched unless the source text actually changed.

## Scope

- **Primary scope: `first-hand-snippets` only.** This mirrors the evidentiary
  rule in `CLAUDE.md` — only direct witness testimony counts as first-hand.
  These entities go under the top-level key `entity-types`.
- Entities that only appear in `general-reputation`, `observation-list`, or
  `personal-musings` (e.g. the wolves/bears/boars musing, or Étienne Hordal in
  the general-reputation texts) go under a separate top-level key,
  `context-only-entity-types`, each instance tagged with a `section` field.
  **Never merge the two** — a reader must be able to tell at a glance which
  entities are directly attested and which are context/hypothesis.
- `candidate-locations` and `object-model` are not sources of entities; they
  don't name animate beings.

## What counts as an "entity type" and an "instance"

- **Entity type** = the vocabulary word/role/title the source text itself
  uses: an occupation (`laboureur`, `prêtre`, `charron`, `couvreur`,
  `marguillier`, `tabellion`, `cultivateur`), a title (`seigneur`, `dame`,
  `chevalier`, `demoiselle`, `marraine`, `parrain`, `oncle`), a relational
  descriptor a deposition header itself uses (`voisin`, `ami d'enfance`), a
  supernatural class (`fée`, `sainte`, `esprit malin`), an afflicted class
  (`fiévreux`, `malade`), or a generic age/sex class (`enfant`, `jeune fille`,
  `jeune homme`, `fillette`, `garçon`). Don't invent a type the text doesn't
  itself use as a label.
- **Instance** = one specific being of that type.
  - If the text gives a proper name (a witness's own name, or a person named
    within a quote — e.g. "Pierre de Bourlemont"), use it verbatim.
  - If the text only speaks of the type collectively/anonymously (e.g. "les
    fées", "les fiévreux", "les jeunes filles" as an unnamed group), record
    **one** instance for that type with `name: "(collective, unnamed)"` —
    never fabricate an individual name to fill the slot.
  - A single individual may legitimately be an instance of more than one
    type (e.g. Pierre Gravier is named first-hand as both `chevalier` and
    `seigneur`; a deponent may be both their stated occupation and, in the
    same header, `ami d'enfance`). Don't force one type per person.

## Rules

1. **Never fabricate.** No name, occupation, or relationship that isn't
   stated in the text. When in doubt, use the collective/unnamed instance
   instead of guessing an individual.
2. **Cite everything.** Every instance carries `document-reference` (and
   `from` when the snippet itself names the witness/speaker) — same
   citation discipline as `breakout/combined.md`.
3. **Quote verbatim.** Include a short `quote` field with the exact source
   text (original French for first-hand/general-reputation material — do
   not translate or paraphrase; personal-musings notes that are themselves
   written in English are quoted as written).
4. **Disambiguate identically-named people.** This corpus reuses names
   across different individuals (e.g. more than one "Béatrice", more than
   one "Jeanne/Jeannette" beside Jeanne d'Arc herself). Make the `name`
   field descriptive enough to tell them apart (e.g. "Béatrice, femme du
   seigneur Pierre de Bourlemont" vs. "Béatrice, veuve de Thévenin
   d'Estellin"). Never silently collapse two mentions into one person unless
   a snippet itself identifies them as the same — if you suspect two
   mentions are the same real person but no snippet states it, add a `note`
   field flagging the possibility instead of merging silently (this is a
   judgment call per the project's evidentiary rules and must be flagged,
   not resolved quietly).
5. **Keep first-hand and non-first-hand strictly separate** — different
   top-level keys, never mixed into the same instance list.
6. When updating an existing file, add new types/instances; don't restructure
   or drop existing entries unless the underlying source text changed.

## Steps

1. Read `prompt/tree-prompt.yaml` in full.
2. If the target output file exists, read it too, to preserve stable
   structure and avoid re-deriving unchanged entries from scratch.
3. Walk `first-hand-snippets` snippet by snippet. For every animate being
   named or described (by role, title, occupation, or proper name), record
   its type and instance with citation, per the rules above.
4. Merge duplicate mentions of the *same, unambiguously identified* person
   across snippets (e.g. a witness's occupation from their own deposition
   header, referenced again elsewhere) into one instance with multiple
   citations, rather than double-listing them.
5. Repeat for `general-reputation`, `observation-list`, and
   `personal-musings`, placing results under `context-only-entity-types`
   with a `section` field per instance.
6. Write the YAML with two top-level keys, `entity-types` and
   `context-only-entity-types`. Alphabetize type names within each for
   stability; list instances in first-appearance order within a type.
7. Save to the target path. Summarize to the user exactly what changed (new
   types, new instances, or nothing) if this was an update.

## Output shape

```yaml
entity-types:
  <type-name>:
    - name: <proper name, or "(collective, unnamed)">
      document-reference: <ref>
      from: <witness/speaker name, if the snippet gives one>
      quote: "<verbatim excerpt>"
      note: <only if a judgment call needs flagging>
context-only-entity-types:
  <type-name>:
    - name: <...>
      section: general-reputation | observation-list | personal-musings
      document-reference: <ref, if any>
      quote: "<verbatim excerpt>"
```

Do not fabricate entities not present in `prompt/tree-prompt.yaml` — when in
doubt, use the collective/unnamed instance and say so.
