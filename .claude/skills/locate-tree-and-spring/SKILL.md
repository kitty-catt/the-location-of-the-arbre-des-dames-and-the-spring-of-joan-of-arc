---
name: locate-tree-and-spring
description: Produce the retired-inspector's best-guess geolocation for the tree and its spring from the full current state of prompt/tree-prompt.yaml — first-hand-snippets, candidate-locations, obervation-list, and personal-musings — following the user-prompt's process/output/performance instructions. Use whenever asked for the best-shot location of the tree or spring, or after a new personal-musing or candidate-location is added that could change the call.
---

# Locate tree and spring

Re-derive the best-supported geolocation (plus a short description) for the
tree (L'Arbre des Dames/Fées/Pucelle) and its spring, using everything
currently in `prompt/tree-prompt.yaml` — not just `first-hand-snippets`. This
is the investigative-hypothesis task defined by the file's own `user-prompt`
section, which explicitly asks to reason from `candidate-locations` and the
"practicalities" musings — unlike the first-hand-only `evidence-diagram`
skill, which must ignore all of that.

Arguments (`$ARGUMENTS`, optional): none expected. Always re-read the whole
file fresh rather than reusing a cached answer — a single newly added
personal-musing or candidate-location can flip the result (e.g. the musing
that the saints spoke "at the spring, not the tree" is what separated the
two sites in a prior run).

## What to read, every time

1. `user-prompt` — re-read `process-description`, `output-description`, and
   `performance-description` in full and follow them literally: give Jeanne
   D'Arc and the nullification witnesses more credibility than
   condemnation-trial assertions, consider the practicalities named there,
   and give a single geolocation plus a radius per object.
2. `personal-musings` — the author's own inferences, and the main lever for
   this task (unlike `evidence-diagram`, which must ignore them entirely).
   Each musing can rule out a candidate, force a separation between two
   entities, or justify an offset. Re-check every musing currently listed
   against the candidate guess — a newly added one can overturn a previous
   answer.
3. `obervation-list` — map/LiDAR-derived observations, keyed by source
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

For each of the two objects (tree, spring):

- **Short description** — one line, grounded in first-hand wording (e.g. "on
  the ridge road, Bourlemont land, beside the road to Neufchâteau").
- **Geolocation** — lat/lon, naming the `candidate-locations` anchor used, or
  marked "estimated" if interpolated.

Below both: one line per judgment call that drove the current guess (rule
5) — omit this section only when the guess is an uncontested
candidate-location with no interpolation or musing-driven offset involved.

## Output format (long form, on request)

Follow `output-description` literally: a table listing geolocation,
confidence score, and search radius for both objects, plus the "notes of a
seasoned police inspector" reflecting on what modern investigative methods
would have added, per `performance-description`.
