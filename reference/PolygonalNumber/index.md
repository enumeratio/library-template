---
name: PolygonalNumber
domain: Number theory
summary: "The n-th polygonal number with a given number of sides: triangular numbers unless `sides` says otherwise."
references:
  - system: wolfram
    identity: PolygonalNumber
  - system: oeis
    identity: A000217
    relation: partial
    note: the triangular numbers, PolygonalNumber's default
  - system: wikipedia
    identity: Polygonal number
definition:
  signature: "(n: integer, sides: integer?) -> integer"
  body: ((sides - 2) * n^2 - (sides - 4) * n) / 2
  defaults:
    sides: "3"
---
