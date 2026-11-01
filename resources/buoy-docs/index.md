---
title: "RandomKit"
summary: "Owned reproducible noncryptographic generators and sampling."
category: "Libraries"
order: 0
keywords: ["random", "seed", "MT19937", "sampling"]
related: ["api.md", "getting-started.md", "typed-api.md"]
---

# RandomKit

Import `randomkit`. Every `RandomGenerator` has explicitly seeded MT19937 state.
No global RNG is read or reseeded. This is noncryptographic randomness.
See [API contracts](api.md), [getting started](getting-started.md), and the
[typed API](typed-api.md). Seeding uses only the low 32 bits of an Integer;
different seeds do not imply independent streams.
