---
name: PyramidalNumber
domain: Number theory
summary: "The n-th pyramidal number with a polygonal base: the sum of the first n polygonal numbers with that many sides."
references:
  - system: oeis
    identity: A000292
    relation: partial
    note: the tetrahedral numbers, the default
  - system: wikipedia
    identity: Pyramidal number
definition:
  signature: "(n: integer, sides: integer?) -> integer"
  body: Sum(enumeratio.PolygonalNumber(k, sides), (k, 1, n))
  defaults:
    sides: "3"
---
