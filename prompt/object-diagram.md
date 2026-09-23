# Object diagram — arbre des Dames and its surroundings

Nodes are the 13 entries of `object-model` in `tree-prompt.yaml`. Each line (edge)
is labeled with the number(s) of the relation(s) it represents — see
`object-relations.md` for what each number means and which source grounds it.
Where more than one relation applies to the same line, the numbers are
comma-separated.

```mermaid
graph TD
    N1["L'Arbre des dames"]
    N2["Chapelle de Notre-Dame de Domremy"]
    N3["Hordal chapel"]
    N4["Basilique"]
    N5["Fontaine fiévreux"]
    N6["Fontaine des Groseilliers"]
    N7["The road to Neufchâteau"]
    N8["Vineyard"]
    N9["Estate boundary"]
    N10["The Bois Chenu"]
    N11["The slope to the top of the Bois Chenu"]
    N12["The valley"]
    N13["The river Meuse"]

    N1 -- "1" --- N7
    N1 -- "2" --- N10
    N1 -- "3" --- N8
    N1 -- "4,5" --- N2
    N1 -- "6" --- N9
    N1 -- "7" --- N5
    N1 -- "8" --- N6
    N2 -- "9" --- N3
    N3 -- "10" --- N4
    N4 -- "11" --- N10
    N4 -- "12" --- N5
    N4 -- "13" --- N11
    N5 -- "14" --- N11
    N6 -- "15" --- N12
    N6 -- "16" --- N13
    N7 -- "17" --- N10
    N7 -- "18" --- N12
    N8 -- "19" --- N11
    N8 -- "20" --- N6
    N9 -- "21" --- N10
    N10 -- "22" --- N11
    N11 -- "23" --- N12
    N12 -- "24" --- N13
```
