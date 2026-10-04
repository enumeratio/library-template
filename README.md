# library-template

A template [enumeratio](https://enumeratio.dev) library: figurate numbers, written in Epsil. Copy
it to start a library of your own.

| symbol | |
| --- | --- |
| `enumeratio.PolygonalNumber(n, sides: 3)` | the n-th polygonal number; triangular by default |
| `enumeratio.CenteredPolygonalNumber(n, sides: 3)` | the n-th centered polygonal number |
| `enumeratio.PyramidalNumber(n, sides: 3)` | the sum of the first n polygonal numbers |
| `enumeratio.StarNumber(n)` | the n-th star number, 6n(n − 1) + 1 |

## Using it

The library is its GitHub repository, versioned by its tags; the owner is its namespace. A
host reads it through `@enumeratio/manifest/libraries`, over jsDelivr, fetching only the
symbols an expression uses:

```ts
import { parseExpression } from "@enumeratio/formats/expression";
import { createRegistryResolver } from "@enumeratio/manifest";
import { catalog, githubHost } from "@enumeratio/manifest/libraries";

const resolver = createRegistryResolver(catalog(["enumeratio/library-template@0.1.0"], { host: githubHost() }));
const { expression } = await resolver.ensure(ce, parseExpression("enumeratio.PolygonalNumber(4, sides: 5)").json);
ce.box(expression).evaluate(); // 22
```

It's a plain JavaScript library too: `dist/index.js` exports `declare(ce)`, which declares every
symbol into a compute-engine.

## Writing one

Each symbol is a folder, `reference/<Name>/`, holding what you write:

- `index.md`: front matter (name, summary, the `references` other sources give it, and its
  `definition`) and a markdown body. The definition is Epsil:

  ```yaml
  definition:
    signature: "(n: integer, sides: integer?) -> integer"
    body: Sum(enumeratio.PolygonalNumber(k, sides), (k, 1, n))
    defaults:
      sides: "3"
  ```

  The signature names each parameter (`?` makes it an option), the body uses them, and a
  default is Epsil too. Packing pins every other symbol the body uses: this library's own, and
  those of the libraries installed beside it; a name nothing installed serves goes in
  `requires`, with its pin.
- `examples.tsv`: one row per example, each with a stable id; they're the install check, so a
  definition whose examples fail isn't declared;
- `notation.json` (optional): how it's written, as box templates and LaTeX triggers;
- `examples.values.<system>.tsv` (optional): each example as another system writes and
  answers it, for the systems `package.json`'s `enumeratio.mappings` names.

Packing writes the rest (`reference/index.json`, each symbol's `definition.json`, `examples.json`
and `mappings.json`, and `dist/`), which you commit, since a tag serves them as they are:

```sh
node <enumeratio>/packages/manifest/scripts/pack-library.ts .
```

with a checkout of [enumeratio/enumeratio](https://github.com/enumeratio/enumeratio). In CI, the
system's action packs it and checks what's committed is current:

```yaml
- uses: actions/checkout@v4
- uses: enumeratio/enumeratio/.github/actions/pack-library@<ref>
```

The ref is the system this library is written against: pin it, and move it when you move to a
newer system. Give the action `since:` (or the script `--since`) a checkout of the previous
version to check that the version number says at least what changed.
