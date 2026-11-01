# Generate reproducible random sequences and samples

Owned, explicitly seeded, reproducible noncryptographic MT19937 generators in pure
Buoy. Import `randomkit` from `randomkit-0.1.0.buipkg`. No dependencies, FFI, entropy
source, or calls to the built-in global `Random` namespace are used by the package.
See [the standalone example](examples/randomkit/main.bui),
[API reference](resources/buoy-docs/api.md), and [provenance](PROVENANCE.md).

Each `RandomGenerator(seed)` owns its 624-word state. Seeds are mapped modulo
2^32, including negative seeds, then initialized by the established 2002
`init_genrand` recurrence. Thus seeds differing by a multiple of 2^32 intentionally
produce identical streams. The raw stream and draw consumption are frozen for
version 0.1.0. Distinct seeds carry no mathematical stream independence guarantee.
This package is unsuitable for keys, tokens, passwords, lotteries, or security.
Use the platform Crypto APIs for security-sensitive randomness.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add randomkit --git https://github.com/BuoyBay/randomkit.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## Example

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

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer randomkit
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```

The generated agent files and editor settings stay local and are ignored by Git.

## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
