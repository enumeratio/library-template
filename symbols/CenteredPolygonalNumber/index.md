---
name: CenteredPolygonalNumber
domain: Number theory
summary: "The n-th centered polygonal number with a given number of sides: a dot, then rings of `sides` dots each."
references:
  - system: oeis
    identity: A005448
    relation: partial
    note: the centered triangular numbers, the default
  - system: wikipedia
    identity: Centered polygonal number
definition:
  signature: "(n: integer, sides: integer?) -> integer"
  body: sides * n * (n - 1) / 2 + 1
  defaults:
    sides: "3"
---
