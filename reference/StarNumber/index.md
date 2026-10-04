---
name: StarNumber
domain: Number theory
summary: "The n-th star number: a centered hexagram, 6n(n - 1) + 1."
references:
  - system: oeis
    identity: A003154
  - system: wikipedia
    identity: Star number
definition:
  signature: "(n: integer) -> integer"
  body: 12 * Binomial(n, 2) + 1
---
