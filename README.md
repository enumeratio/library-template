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

Each symbol is a folder, `symbols/<Name>/`, holding what you write:

- `index.md`: front matter (name, summary, the `references` other sources give it) and a
  markdown body;
- `examples.tsv`: one row per example, each with a stable id; they're the install check, so a
  definition whose examples fail isn't declared;
- `definition.json`: the signature (parameter names, `?` for an option), the body (an Epsil
  `Function`, as MathJSON), `requires` (the pin of every other library's symbol it uses) and
  `defaults` for its options;
- `notation.json` (optional): how it's written, as box templates and LaTeX triggers;
- `examples.values.<system>.tsv` (optional): each example as another system writes and
  answers it, for the systems `package.json`'s `enumeratio.mappings` names.

Packing writes the rest (`symbols/index.json`, each symbol's `examples.json` and
`mappings.json`, and `dist/`), which you commit, since a tag serves them as they are:

```sh
node <enumeratio>/packages/manifest/scripts/pack-library.ts .
```

with a checkout of [enumeratio/enumeratio](https://github.com/enumeratio/enumeratio). CI checks
what's committed is current. Give `--since <previous version's checkout>` to check that the
version number says at least what changed.
