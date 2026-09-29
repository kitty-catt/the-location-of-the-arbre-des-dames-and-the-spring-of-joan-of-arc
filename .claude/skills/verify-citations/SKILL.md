---
name: verify-citations
description: Audit a breakout/analysis file (or all of them) against prompt/tree-prompt.yaml to catch claims presented as first-hand testimony that are actually secondary, inferred, or unsupported. Use before trusting or publishing any analysis derived from the trial evidence.
---

# Verify citations

Check that every factual claim or quote in one or more analysis files is
actually backed by `prompt/tree-prompt.yaml`, and correctly categorized as
first-hand vs. not. This catches the exact failure mode the evidentiary rules
in `CLAUDE.md` exist to prevent: an inferred or secondary claim quietly being
treated as direct witness testimony.

Arguments (`$ARGUMENTS`, optional): one or more file paths to check. If
omitted, check every file in `breakout/`.

## What to check, per claim or quote found in the target file(s)

1. **Does it exist in `prompt/tree-prompt.yaml` at all?** Search
   `first-hand-snippets`, `general-reputation`, `observation-list`, and
   `personal-musings` for the source text. If it doesn't appear anywhere,
   flag it as **unsupported/fabricated**.
2. **Is it correctly categorized?** If the target file presents the claim as
   first-hand witness testimony (or draws a diagram edge / stated fact from
   it) but the matching text actually lives in `general-reputation`,
   `observation-list`, or `personal-musings` — flag it as
   **miscategorized (secondary treated as first-hand)**.
3. **Is the attribution correct?** Compare the `document-reference` and
   witness name (the `from` field, or a name embedded in the snippet text)
   cited in the target file against what `tree-prompt.yaml` actually records.
   Flag any mismatch as **misattributed**.
4. **Is the relationship actually stated, or inferred?** For diagram edges or
   claims of the form "X is near/belongs to/leads to Y," confirm the cited
   snippet's text directly states that relationship. If the target file's
   claim goes further than the quoted text supports (e.g. treating adjacency
   language as identity, or chaining two separate snippets into one
   relationship neither states alone), flag it as **overreach**.
5. **Quote fidelity.** If a quote is reproduced, confirm it matches the
   source text in `tree-prompt.yaml` verbatim (period French, no
   paraphrase/translation substituted for it). Flag any drift as **quote
   altered**.

## Output format

Report findings as a table, most severe first (unsupported > miscategorized
> misattributed > overreach > quote altered), each row with:

- the file and line/section where the claim appears
- the claim as stated in the target file
- what `tree-prompt.yaml` actually supports (quote the relevant snippet and
  its section)
- the flag type from above

If a file has zero findings, say so plainly — don't pad the report. Do not
edit the target files yourself; this skill only reports. If the user wants
fixes applied after reviewing the report, make them in a separate step.
