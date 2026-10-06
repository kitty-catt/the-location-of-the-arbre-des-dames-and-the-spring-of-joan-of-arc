---
name: extract-objects
description: Extract inanimate objects (trees, fountains/springs, roads, woods, and other named physical things) described in first-hand testimony in prompt/tree-prompt.yaml into a structured prompt/objects.yaml, grouped by object type with each type's array of named/described instances. Use when asked to catalog, extract, or update the physical objects, places, or items named in the source material.
---

# Extract objects

Build or update a catalog of inanimate objects named or described in
`prompt/tree-prompt.yaml`'s `first-hand-snippets`, grouped by object type —
e.g. type `arbre` (tree) has instances `l'arbre des Dames` and `l'arbre des
Fées`.

Arguments (``, optional): an output path (default
`prompt/objects.yaml`). If it already exists, treat this as an update —
read it first and fold in anything new rather than starting over; keep
existing types/instances untouched unless the source text actually
changed.

## Scope

- **Primary scope: `first-hand-snippets` only.** This mirrors the
  evidentiary rule in `CLAUDE.md` and the sibling `extract-entities` skill —
  only direct witness testimony counts as first-hand. These objects go
  under the top-level key `object-types`.
- Objects that only appear in `general-reputation`, `observation-list`, or
  `personal-musings` (e.g. the "fontaine aux Groselles" from the
  Napoleonic-cadastre `observation-list` entry, or Montaigne's "arbre de la
  Pucelle" aside) go under a separate top-level key,
  `context-only-object-types`, each instance tagged with a `section`
  field. **Never merge the two** — a reader must be able to tell at a
  glance which objects are directly attested and which are context/
  hypothesis.
- `candidate-locations` is not a source of objects — it is the author's own
  geolocation hypotheses (lat/lon, distances, elevations), not testimony.
- `object-model` is a different thing entirely: it's the fixed vocabulary
  the `evidence-diagram` skill is restricted to when drawing a diagram. This
  skill is **not** restricted to it — any inanimate object a first-hand
  snippet names or describes counts, whether or not it appears in
  `object-model`. Treat `object-model` only as an optional cross-check for
  completeness, never as a filter on what to extract.

## What counts as an "object type" and an "instance"

- **Object type** = the common-noun word (or its clear Latin/archaic-French
  equivalent) the source text itself uses for the kind of object: `arbre`
  (tree), `fontaine` (fountain/spring), `chemin` (road), `bois` (wood),
  `guirlande` (garland — including "chappeaulx"/"couronnes" used for the
  same flower/herb wreaths), `pain` (bread), `vin` (wine), `oeuf` (egg),
  `noix` (walnut), `nappe` (tablecloth), `cierge` (candle), `croix`
  (cross), `image` (devotional image/statue), `mandragore` (mandrake),
  `coudrier` (hazel tree), `maison`/`huis` (house/doorway), `mai`/`beau mai`
  (maypole bough), and so on. Don't invent a type the text doesn't itself
  use as a label; don't split one word into two types or merge two
  different words into one.
- **Required coverage.** Whatever noun(s) the source text uses for a tree,
  a fountain/spring, and a road must each get their own type without
  exception, however sparsely attested — these are central to the
  investigation. Note that period French has no separate word for
  "fountain" vs. "spring"; both are "fontaine" (or Latin "fons"/"fontem").
  A single type named `fontaine` (described as "fountain/spring" in its
  own notes) correctly covers both — don't force two separate types apart
  where the text itself doesn't distinguish them.
- **Instance** = one specific object of that type, identified by whatever
  name or description the snippet actually gives (e.g. "l'arbre des
  Dames", "la Fontaine-des-Groseilliers", "le grand chemin par lequel on va
  à Neufchâteau"). If the text speaks only generically/anonymously of the
  type (e.g. "une fontaine", unqualified, with no proper name), record
  **one** instance for that mention with `name: "(unspecified, no proper
  name given)"` — never fabricate a proper name to fill the slot.
- **Don't silently merge or silently split synonyms.** The same real-world
  object is often named differently across snippets (e.g. the tree is also
  called "l'arbre des Fées", "le Fou", "aux Loges-les-Dames"). Keep each
  literal name as its own instance by default. When a snippet *itself*
  states that two names refer to the same thing (e.g. Jeanne's "appelé
  l'arbre des Dames; d'autres l'appelaient l'arbre des Fées"), add a `note`
  on both instances cross-referencing the other and citing that snippet —
  make the stated equivalence visible rather than collapsing the two
  entries into one or leaving the link undocumented. When you only
  *suspect* two differently-named mentions are the same object but no
  snippet says so, add a `note` flagging the possibility — this is a
  judgment call per the project's evidentiary rules and must be flagged,
  not resolved quietly, exactly as `extract-entities` does for
  identically-named people.

## Rules

1. **Never fabricate.** No name, description, or location that isn't
   stated in the text. When in doubt, use the unspecified instance instead
   of guessing a proper name.
2. **Cite everything.** Every instance carries `document-reference` (and
   `from` when the snippet itself names the witness/speaker) — same
   citation discipline as `breakout/combined.md` and `prompt/entities.yaml`.
3. **Quote verbatim.** Include a short `quote` field with the exact source
   text (original French/Latin for first-hand material — do not translate
   or paraphrase; `personal-musings`/English notes are quoted as written).
4. **Disambiguate similarly-described objects.** This corpus reuses
   generic nouns across plausibly-different real things (e.g. more than one
   unqualified "une fontaine" that may or may not be the same spring as
   "fontaine des Rains" / "fontaine fiévreux" / "Fontem Rannorum"). Never
   silently collapse two mentions into one object unless a snippet itself
   identifies them as the same — add a `note` instead.
5. **Keep first-hand and non-first-hand strictly separate** — different
   top-level keys, never mixed into the same instance list.
6. **Always cover tree, fountain/spring, and road types** when the source
   text names or describes any instance of them, even a single sparse
   mention — don't drop these for brevity.
7. When updating an existing file, add new types/instances; don't
   restructure or drop existing entries unless the underlying source text
   changed.

## Steps

1. Read `prompt/tree-prompt.yaml` in full.
2. If the target output file exists, read it too, to preserve stable
   structure and avoid re-deriving unchanged entries from scratch.
3. Walk `first-hand-snippets` snippet by snippet. For every inanimate
   object named or described (by its own common-noun type and whatever
   name/description accompanies it), record its type and instance with
   citation, per the rules above. Make sure every tree, fountain/spring,
   and road mention is captured.
4. Merge duplicate mentions of the *same, unambiguously identified* object
   across snippets (e.g. the same named fountain referenced by several
   witnesses) into one instance with multiple citations, rather than
   double-listing them — mirroring how `extract-entities` merges repeat
   mentions of the same person. Parallel French/Latin/archaic-French
   transcriptions of the *same* deposition passage count as one citation
   with the alternate wordings noted, not separate instances.
5. Repeat for `general-reputation`, `observation-list`, and
   `personal-musings`, placing results under `context-only-object-types`
   with a `section` field per instance.
6. Write the YAML with two top-level keys, `object-types` and
   `context-only-object-types`. Alphabetize type names within each for
   stability; list instances in first-appearance order within a type.
7. Save to the target path. Summarize to the user exactly what changed (new
   types, new instances, or nothing) if this was an update.

## Output shape

```yaml
object-types:
  <type-name>:
    - name: <proper name/description, or "(unspecified, no proper name given)">
      document-reference: <ref>
      from: <witness/speaker name, if the snippet gives one>
      quote: "<verbatim excerpt>"
      note: <only if a judgment call or cross-reference needs flagging>
context-only-object-types:
  <type-name>:
    - name: <...>
      section: general-reputation | observation-list | personal-musings
      document-reference: <ref, if any>
      quote: "<verbatim excerpt>"
```

Do not fabricate objects not present in `prompt/tree-prompt.yaml` — when in
doubt, use the unspecified instance and say so. Always include `arbre`
(tree), `fontaine` (fountain/spring), and `chemin` (road) types whenever
the first-hand testimony names or describes any instance of them.
