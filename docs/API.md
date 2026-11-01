# RandomKit 0.1.0

Owned, explicitly seeded, reproducible noncryptographic MT19937 generators in pure
Buoy. Import `randomkit` from `randomkit-0.1.0.buipkg`. No dependencies, FFI, entropy
source, or calls to the built-in global `Random` namespace are used by the package.
See [the standalone example](../examples/randomkit/main.bui),
[API reference](../resources/buoy-docs/api.md), and [provenance](../PROVENANCE.md).

Each `RandomGenerator(seed)` owns its 624-word state. Seeds are mapped modulo
2^32, including negative seeds, then initialized by the established 2002
`init_genrand` recurrence. Thus seeds differing by a multiple of 2^32 intentionally
produce identical streams. The raw stream and draw consumption are frozen for
version 0.1.0. Distinct seeds carry no mathematical stream independence guarantee.
This package is unsuitable for keys, tokens, passwords, lotteries, or security.
Use the platform Crypto APIs for security-sensitive randomness.

`NextInteger(lower, upper)` has inclusive signed 64-bit bounds and uses integer
bit rejection even for the entire signed range. It introduces no modulo or
floating-point reduction bias; this describes the sampling transformation under
uniform source words, not a claim that a finite deterministic generator is true
randomness. `NextDouble` uses the standard 53-bit grid in [0,1). Normal and
exponential transforms are floating-point approximations, not continuous exact
samples. Bernoulli probabilities are resolved against that finite grid.

Generators are mutable reference objects, not thread-safe. Assigning one to another
variable aliases it; `Clone()` produces an independent copy at the same position.
State export/restore copies arrays and is intended for reproducibility, not secure
storage. Restored states are structurally validated, not certified reachable from
a seed. Normal output has no cached spare. No automatic/OS seeding, jumping,
splitting, parallel stream guarantees, or cryptographic properties are provided.

Maintainers run the private validation suite before release. See the repository README for public build instructions.
