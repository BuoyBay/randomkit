---
title: "RandomKit API"
summary: "Precise stream, interval, distribution and state contracts."
category: "Reference"
order: 20
keywords: ["MT19937", "state", "inclusive", "rejection"]
symbols: ["RandomGenerator", "RandomGenerator.NextWord", "RandomGenerator.NextDouble", "RandomGenerator.NextInteger", "RandomGenerator.NextNormal", "RandomGenerator.NextExponential", "RandomGenerator.NextBernoulli", "RandomGenerator.Clone", "RandomGenerator.ExportState", "RandomGenerator.RestoreState", "Shuffle", "Sample", "Choice"]
related: ["index.md", "getting-started.md", "typed-api.md"]
---

# Contracts

| API | Behavior |
| --- | --- |
| Constructor(seed) | Seed modulo 2^32, then 2002 MT19937 init_genrand; seed is mandatory |
| NextWord() | Tempered 32-bit word as nonnegative Integer; consumes one word |
| NextDouble() | `(word1 >> 5)*2^26 + (word2 >> 6)`, divided by 2^53; [0,1) |
| NextInteger(lower, upper) | Inclusive signed range, including full Integer domain; reversed bounds throw; singleton consumes nothing |
| NextNormal(mean=0, sd=1) | Finite mean and finite sd >= 0; sd=0 returns mean without draws; otherwise Box-Muller cosine branch |
| NextExponential(rate) | Finite rate > 0; `-log(u)/rate` with zero uniforms retried |
| NextBernoulli(p) | Finite p in [0,1]; endpoints consume nothing; otherwise NextDouble() < p |
| ExportState() | Detached 626-entry Integer array `[1, index, word0, ..., word623]` |
| RestoreState(state) | Version 1, index 0..624, words 0..2^32-1, at least one nonzero word; validate before mutation; detached copy |
| Clone() | New generator with identical current state, no shared mutable array |
| Shuffle(generator, items) | Descending Fisher-Yates in place, inclusive index [0,i] |
| Sample(generator, items, count) | Ordered sample of positions without replacement; count 0..length; copy input then swap i with inclusive [i,n-1] |
| Choice(generator, items) | Nonempty input required; inclusive index [0,n-1] |

Invalid inputs throw InvalidArgumentException before consuming randomness.
Sampling duplicates are allowed when different input positions contain equal
values. Generic operations reject Nil generators, even on empty arrays. Shuffle
accepts fixed arrays since it only overwrites slots. Empty/singleton shuffles,
empty samples, and singleton choices consume no words. Sample uses O(n) storage
and O(n+count) work; shuffle O(n) work, O(1) extra storage.

Integer rejection computes unsigned distance upper-lower as two nonnegative
32-bit limbs, masks words to its bit length, and rejects values above that
distance. It draws high word first when two are required; otherwise only the
low word. It adds the accepted offset to the lower signed rank without signed
overflow. Exact draw counts vary with rejection.

Normal retries a zero first uniform, draws a second uniform, and uses no spare
cache. Exponential also retries zero. Uniforms are finite-grid samples; tail
support is bounded by that grid and the generator, not an ideal unbounded law.
For extreme finite parameters normal/exponential arithmetic can overflow to
infinity or underflow to zero. Neither API promises finite output for all finite
parameters. Transcendental rounding may differ between native platforms; only
raw words, integer transforms, and 53-bit uniforms have an algorithmic exact
reproducibility contract. No non-Linux execution is claimed.

Generators have no locking. Exported state is not authenticated or secret.
Structural validation cannot establish that a state is reachable from a seed or
has favorable statistical properties. There is no stream independence guarantee.
