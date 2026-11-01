---
title: "RandomKit getting started"
summary: "Construct a seeded generator and choose or sample array items."
category: "Guides"
order: 10
keywords: ["seed", "choice", "shuffle", "sample"]
related: ["index.md", "api.md", "typed-api.md"]
---

# Getting started

Place `randomkit-0.1.0.buipkg` in the consumer's `packages/` directory and import
`randomkit`. Construct `New RandomGenerator(5489)`; its first `NextWord()` is
3499211612. Always provide a seed. There is no ambient entropy dependency.

`Shuffle(generator, items)` mutates an array; `Sample(generator, items, count)`
returns a new array; `Choice(generator, items)` returns one element. These generic
functions infer the array's element type. Copies of arrays retain element object
identity; class instances are not deep-cloned.

Use `generator.Clone()` for a second generator at the exact same stream position.
An ordinary variable assignment shares the object. ExportState returns a detached
array; RestoreState validates and copies it. For a complete compilable example,
see `examples/randomkit/main.bui` in the workspace.


Save the following as `main.bui` in your consumer project. Package archives and any dependencies must be installed as described above.

```buoy
Import randomkit

Sub Main()
    Dim generator As New RandomGenerator(5489)
    Print generator.NextWord()
    Print generator.NextInteger(-10, 10)
    Print generator.NextDouble()
    Dim cards As Array Of String = ["ace", "king", "queen"]
    Shuffle(generator, cards)
    Print Choice(generator, cards)
    Dim hand As Array Of String = Sample(generator, cards, 2)
    Print Array.Count(hand)
    Dim copy As RandomGenerator = generator.Clone()
    Print copy.NextWord() = generator.NextWord()
End Sub
```
