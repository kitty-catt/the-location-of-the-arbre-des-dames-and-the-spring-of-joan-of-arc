---
name: locate-tree-and-spring
description: Produce the retired-inspector's best-guess geolocation for the tree and its spring from the full current state of prompt/tree-prompt.yaml — first-hand-snippets, candidate-locations, observation-list, and personal-musings — following the process/output/performance instructions embedded in this skill. Use whenever asked for the best-shot location of the tree or spring, or after a new personal-musing or candidate-location is added that could change the call.
---

# Locate tree and spring

Re-derive the best-supported geolocation (plus a short description) for the
tree (L'Arbre des Dames/Fées/Pucelle) and its spring, using everything
currently in `prompt/tree-prompt.yaml` — not just `first-hand-snippets`. This
is the investigative-hypothesis task defined by the "Task definition" section
below, which explicitly asks to reason from `candidate-locations` and the
"practicalities" musings — unlike the first-hand-only `evidence-diagram`
skill, which must ignore all of that.

Arguments (`$ARGUMENTS`, optional): none expected. Always re-read the whole
file fresh rather than reusing a cached answer — a single newly added
personal-musing or candidate-location can flip the result (e.g. the musing
that the saints spoke "at the spring, not the tree" is what separated the
two sites in a prior run).

## Task definition (source of truth)

### Process
- Pretend you are a retired modern-day police inspector with some time to spare.
- The objective is a best guess for where to search for the remnants of the spring and the tree of the Pucelle.
- Read the `first-hand-snippets`.
- Read the `candidate-locations` and use them as a starting point for your musing.
- Consider the practicalities of children visiting the tree.
- Consider the practicalities of the Lady de Bourlemont visiting the tree with her demoiselles.
- Consider the practicalities of the Lord and Lady de Bourlemont visiting the tree with the mother of the Lord, their demoiselles and servants.
- Give the nullification-trial witness accounts, as well as Jeanne D'Arc's, more credibility than other statements.
- Where could a large, socially prominent tree have stood that satisfies all witness accounts simultaneously?
- Where could the spring have stood that satisfies all witness accounts simultaneously?
- Suggest the best candidate location for the tree, then a better one using the `candidate-locations` as the starting point for your calculations.
- Do the same for the spring.
- Give a single geolocation plus a radius for each object.

### Output
- A table in which the inspector lists the geolocation of the tree and spring, a confidence score, and the range around each object where he would search.
- Notes of a seasoned police inspector, living at the AI's knowledge cutoff, who has read the novel: what he thought was right, and what he would have done with modern investigation methods.

### Performance
- Give the best-supported geolocation for the spring and the tree and explain the reasoning.

## What to read, every time

1. The task definition above — follow it literally.
2. `personal-musings` — the author's own inferences, and the main lever for
   this task (unlike `evidence-diagram`, which must ignore them entirely).
   Each musing can rule out a candidate, force a separation between two
   entities, or justify an offset. Re-check every musing currently listed
   against the candidate guess — a newly added one can overturn a previous
   answer.
3. `observation-list` — map/LiDAR-derived observations, keyed by source
   (naudin-map, jollois, Carte de l'état-major, LiDAR scan). Use these for
   terrain/road reasoning: slope, clearing, washout, a narrowing path.
4. `candidate-locations` — the geolocated starting points. Prefer one
   directly when it fits. When the musings/observations place the answer
   between two existing candidates, interpolate linearly between them (as
   was done for the "fountain washout at the plateau edge" entry) and label
   the result an estimate — never present an interpolation as a measured
   fix.
5. `first-hand-snippets` — the hard constraints: who/what is placed where,
   adjacency language ("auprès," "proche"), and named relationships. This
   task is allowed to reason beyond first-hand snippets (into musings and
   candidate-locations), but a guess may never contradict what a first-hand
   snippet directly states.
6. `general-reputation` — secondary sources (Montaigne, 19th-century
   historians). Usable as supporting context, never as a substitute for
   first-hand testimony.

## Rules

1. **`personal-musings` is the primary driver.** Walk through every musing
   currently in the file and check it against the candidate guess. If a
   musing's implication is violated by the current best guess, change the
   guess and name the musing that forced the change.
2. **Don't default to co-locating the tree and spring.** Check for a musing
   or snippet implying separation (e.g. a question's phrasing that
   distinguishes "at the spring" from "at the tree") before deciding whether
   they're the same point or two nearby points.
3. **Anchor to `candidate-locations`; interpolate only when needed, and say
   so.** If no existing candidate fits, interpolate linearly between the two
   nearest candidates along the slope/road and flag it explicitly as an
   estimate.
4. **First-hand snippets are hard limits.** Musings and candidate-locations
   can freely shape the guess, but it may never place the tree or spring
   somewhere a first-hand snippet directly rules out (e.g. off the
   Bourlemont land, or off the road to Neufchâteau).
5. **Flag every judgment call in one line.** State which musing, observation,
   or interpolation drove each choice — don't resolve ambiguity silently.
6. **Default to a short answer.** Use the compact output format below unless
   asked for the full inspector's write-up.

## Output format (default)

A compact table, one row per object (tree, spring):

| Object | Short description | Geolocation | Confidence | Search radius |
|---|---|---|---|---|

- **Short description** — one line, grounded in first-hand wording (e.g. "on
  the ridge road, Bourlemont land, beside the road to Neufchâteau").
- **Geolocation** — lat/lon, naming the `candidate-locations` anchor used, or
  marked "estimated" if interpolated.
- **Confidence / radius** — required by the Output section above.

Below the table: one line per judgment call that drove the current guess
(rule 5) — omit this only when the guess is an uncontested
candidate-location with no interpolation or musing-driven offset involved.

## Output format (long form, on request)

Add the full reasoning (Performance) and the "notes of a seasoned police
inspector" reflecting on what modern investigative methods would have added.
