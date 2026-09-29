# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

There is no code to build, lint, or test. This is a historical-research/documentation project whose actual purpose (stated in `README.md`) is for the author to gain hands-on experience working with AI. The research subject is a "fictional police report" investigating where L'Arbre des Dames (also L'Arbre des Fées / l'Arbre de la Pucelle) and its nearby spring stood, near Domrémy, based on witness testimony from Joan of Arc's condemnation and nullification trials.

Treat this repo as a body of primary-source evidence plus prior analysis to reason over, not as software to run.

## Repository structure

- `prompt/tree-prompt.yaml` — the single source of truth for evidence. Structured YAML containing:
  - `first-hand-snippets`: direct trial testimony quotes (French), each tagged with `document-reference` and sometimes `from` (e.g. Jeanne D'Arc). **This is the only section that counts as first-hand evidence.**
  - `general-reputation`: secondary/later sources (Montaigne, 19th-century historians) — not first-hand.
  - `observation-list`, `personal-musings`: the author's own notes/inferences — not first-hand.
  - `candidate-locations`: specific lat/lon geolocations with distance/elevation data, used as starting points for analysis.
  - `object-model`: the fixed vocabulary of named entities (trees, fountains, roads, land parcels) that any diagram or analysis must draw from.
  - `document-refence-list`: source URLs keyed by name, referenced from snippets via `document-reference`.
  - `user-prompt`: instructions for an LLM roleplaying as a retired police inspector, defining the process/output/performance expectations for the actual analysis task.
- `breakout/*.md` — per-source analysis notes, one per map/document (`naudin.md`, `jollois.md`, `napoleonic.md`, `copernicus.md`), plus `combined.md`, a mermaid object-relationship diagram built strictly from `first-hand-snippets`.
- `images/` — supporting maps and imagery referenced by the breakout files (Napoleonic cadastre, Naudin map, Jollois map, Copernicus moisture maps, LIDAR scan, 1821 etching).
- `README.md` — records the exact prompts used to generate derived artifacts (e.g. the instructions that produced `breakout/combined.md`). When you generate a new derived artifact from a prompt, follow this same pattern: append the prompt you were given to `README.md` next to a link to the output.

## Evidentiary rules (critical — do not violate these when analyzing or diagramming)

These rules were established while building `breakout/combined.md` and apply to any similar analysis:

1. **First-hand only, unless told otherwise.** Only `first-hand-snippets` count as direct evidence. `general-reputation`, `observation-list`, `personal-musings`, and `candidate-locations` are context/hypothesis, not evidence — never cite them as if a witness stated something.
2. **No inference from indirect implication.** A relationship between two entities may only be drawn if a snippet *directly* states it. Do not infer a relationship from proximity, distance math, or plausibility.
3. **Respect the fixed `object-model` vocabulary, and drop isolated nodes.** When diagramming, only include an entity if it is directly named in a first-hand snippet AND has a first-hand-stated relationship to another included entity. Exclude everything else — whether never named first-hand, or named first-hand but never placed in a stated relationship — and say explicitly why each was excluded (see the "Excluded entirely" section pattern in `breakout/combined.md`).
4. **Cite every claim** with its `document-reference` and, where relevant, the deposing witness (`from` or the name in the snippet text).
5. **Flag judgment calls explicitly.** Where a categorization is ambiguous (e.g. whether two mentions of "une fontaine" refer to the same spring), state the judgment call and the textual basis for it, rather than silently resolving it.

## Working conventions

- The witness testimony is in period French; keep quotes verbatim (don't translate/paraphrase) when citing them as evidence, and reference by document + witness name.
- Geolocations in `candidate-locations` use lat/lon in decimal degrees, with distances/elevations given in a mix of meters and feet (ft) as sourced from the original notes — preserve the original units rather than normalizing them.
- Diagram outputs use Mermaid (`graph TD`) syntax embedded in Markdown, with numbered edge labels keyed to a numbered list of supporting quotes below the diagram — follow this format for new diagrams.
