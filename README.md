# Introduction

This repository contains a prompt to that aims to deduce where the tree and the spring of Jeanne D'Arc also known as the Pucelle, or Joan of Arc, the Maid COULD have stood.

The author of this repo places his trust in the work of previous generations to pinpoint these locations. 

The main purpose of this repo is to let the author wrestle with artificial intelligence in order to gain experience with it.


# Method

Relevant snippets from Joan of Arc's condemnation and nullification trail were taken if they relate to the tree or the spring. 

A similar approach was taken with documentation that was know to the author of this repo at the time.

Further instructions on product, process and performance are in the prompt.


# Instruction

1. The [prompt](prompt/tree-prompt.yaml) is posted to an unitialized AI LLM model.
2. The response is documented


# Diligence Statement

In creating this chapter of a fictional police report, I collaborated with Claude, Grok, Copilot and ChatGPT AI to assist a fictional police inspector deduce the whereabouts of the location of the tree and the spring of the Pucelle.

I affirm that all AI-generated and co-created content underwent thorough review and evaluation. The final output accurately reflects my understanding, expertise, and intended meaning. While AI assistance was instrumental in the process, I maintain full responsibility for the content, its accuracy, and its presentation. This disclosure is made in the spirit of transparency and to acknowledge the role of AI in the creation process.

# AI generated object relations based on first hand witness accounts

[click here for the diagram](breakout/combined.md)

```mermaid
graph TD
    classDef archaeological fill:#2ecc71,stroke:#1e8449,color:#000;
    classDef jeanneFievreux fill:#aed6f1,stroke:#2471a3,color:#000;

    N1["L’Arbre des dames"]
    N2["L’Arbre des fées"]
    N3["fontaine des Groseilliers"]
    N4["fontaine fiévreux"]
    N5["fontaine aux Rains"]
    N6["the road to Neufchâteau"]
    N7["the Bourlemont land"]
    N8["un Bois"]
    N9["Les saintes"]
    N10["fiévreux"]
    N11["malades"]
    N12["fées"]
    N13["le curé"]
    N14["Fontem Rannorum"]

    N1 == "5" === N2
    N1 -- "1,2,3,4" --- N3
    N1 == "5,7,29" === N4
    N1 == "6" === N7
    N4 == "7" === N9
    N1 == "30,31,32,34,35,37,38,39,40,41,42,43,46" === N12
    N1 -- "36" --- N13
    N5 -- "36" --- N13
    N4 == "26" === N10
    N1 == "27" === N11
    N1 -- "10,11,12,13,15,16,18,19,20,21,22,23,24,25" --- N5
    N2 -- "17" --- N5
    N1 -- "12,14" --- N6
    N1 -- "14" --- N8
    N1 -- "44,45,47" --- N14

    class N3,N4,N5,N6,N14 archaeological
    class N1,N9,N10 jeanneFievreux
```


# Personal Thoughts

| Object | Observations |
|---|---|
| [naudin-map](breakout/naudin.md)                  | The path from Domremy goes up the ridge and stops at the entry of the shoulder of the hill. Where the path leads to the plateau, there is also a spot where the ridge becomes more narrow. The fountain may have washed out the path and made it smaller. Label this path as the ridge old road.|
| [jollois-map](breakout/jollois.md)                | The vineyard grew on both sides of the ridge road. From the ridge road it is possible to see the new road to Neufchateau on the other side of the river |
| [carte-etat-major](breakout/carte-etat-major.md)  | The ridge road could have been more to the west than the current D53. The ridge road ends stops where the basilique begins. Lidar Scan: there are traces in the landscape from what could have been the fountain washing over the road into the lower road, just north where the plateau begins |
| [napoleonic-cadastre](breakout/napoleonic.md)     | The old ridge road ends stops where the basilique begins. The map shows that South of the basilique the territory of Coussey starts in the Napoleonic times. There is a patch of land next to the river Meuse called la Fontaine aux Groselles |
| [copernicus-map](breakout/copernicus.md)          | - |



# Working with Claude Skills

A Claude Skill is a packaged, reusable set of instructions stored as a
`SKILL.md` file under `.claude/skills/<name>/`. Each one carries a `name` and
`description` in its frontmatter so Claude Code can tell when it applies, and
a body of step-by-step instructions tailored to this repo's evidentiary
rules. Invoke one explicitly by typing `/<name>` (e.g. `/evidence-diagram`)
in Claude Code; Claude may also offer to use one on its own when a request
matches its description.

Skills defined in this repo:

| Skill | Use it to... |
|---|---|
| `/evidence-diagram` | Rebuild the first-hand-only Mermaid diagram (`breakout/combined.md`) after adding witness testimony. |
| `/verify-citations` | Audit a `breakout/` file against `prompt/tree-prompt.yaml` for claims miscategorized as first-hand. |
| `/extract-entities` | Pull named people/supernatural beings/afflicted groups out of `prompt/tree-prompt.yaml` into `prompt/entities.yaml`. |
| `/locate-tree-and-spring` | Produce the retired inspector's best-guess geolocation for the tree and spring from the current `personal-musings`, `candidate-locations`, and `obervation-list` — the hypothesis-building task, as opposed to the strictly-first-hand diagram. |
| `/sync-observations` | Carry new `## Observations` bullets from a `breakout/*.md` map/data file into `prompt/tree-prompt.yaml`'s `obervation-list`, then refresh the matching row of this README's Personal Thoughts table. |

Typical workflow going forward:
1. Add new witness testimony to `prompt/tree-prompt.yaml` (as `first-hand-snippets`, correctly categorized).
2. Run `/evidence-diagram` to fold the new snippets into the diagram — it reads the existing `breakout/combined.md` first so node/edge numbering for unchanged content stays stable, and reports what actually changed.
3. Run `/verify-citations` on the updated file (or the whole `breakout/` folder) as a check before you trust or publish the result — it'll flag anything presented as first-hand that's actually secondary, misattributed, or overreaching beyond what the quote states.
4. When you notice a new fact in a breakout file's `## Observations` section, run `/sync-observations` to carry it into `prompt/tree-prompt.yaml` and this README's Personal Thoughts table together, instead of editing all three by hand.
5. When adding a new `personal-musing` or `candidate-location` (e.g. a map/LiDAR observation), run `/locate-tree-and-spring` to see whether it changes the best-guess location for either object.



