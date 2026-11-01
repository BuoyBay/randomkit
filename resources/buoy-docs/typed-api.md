---
title: "randomkit typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["RandomGenerator"]
dependencies: []
symbols: ["RandomGenerator"]
related: ["api.md", "getting-started.md", "index.md"]
---

# randomkit typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## RandomGenerator

`Public Class RandomGenerator`

Explicitly seeded MT19937; noncryptographic and not synchronized.

### Constructor

`Public Sub Constructor(seed As Integer)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `seed` | `Integer` | `ByVal` | Required |

Returns: no value.

### NextWord

`Public Function NextWord() As Integer`

A raw tempered unsigned 32-bit word represented as Integer.

No parameters.

#### Returns

| Type |
| --- |
| `Integer` |

### NextDouble

`Public Function NextDouble() As Double`

53-bit grid k / 2^53 in [0, 1), consuming two words.

No parameters.

#### Returns

| Type |
| --- |
| `Double` |

### NextInteger

`Public Function NextInteger(lower As Integer, upper As Integer) As Integer`

Inclusive bounds. Integer-only bit rejection, including the whole signed range.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `lower` | `Integer` | `ByVal` | Required |
| `upper` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Integer` |

### NextBernoulli

`Public Function NextBernoulli(p As Double) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `p` | `Double` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### NextExponential

`Public Function NextExponential(rate As Double) As Double`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `rate` | `Double` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Double` |

### NextNormal

`Public Function NextNormal(mean As Double = 0.0, sd As Double = 1.0) As Double`

Box-Muller, no cached spare: two uniform draws per accepted nonzero first draw.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `mean` | `Double` | `ByVal` | `0.0` |
| `sd` | `Double` | `ByVal` | `1.0` |

#### Returns

| Type |
| --- |
| `Double` |

### ExportState

`Public Function ExportState() As Array Of Integer`

Version 1: [1, index, 624 words]. Always returns a detached array.

No parameters.

#### Returns

| Type |
| --- |
| `Array Of Integer` |

### RestoreState

`Public Sub RestoreState(state As Array Of Integer)`

Validate fully before replacing state. All-zero state is rejected.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `state` | `Array Of Integer` | `ByVal` | Required |

Returns: no value.

### Clone

`Public Function Clone() As RandomGenerator`

No parameters.

#### Returns

| Type |
| --- |
| `RandomGenerator` |

## RandomKit functions

### Shuffle

`Public Sub Shuffle Of T(generator As RandomGenerator, items As Array Of T)`

Fisher-Yates in place. The array is mutated; no draws for zero/one item.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `generator` | `RandomGenerator` | `ByVal` | Required |
| `items` | `Array Of T` | `ByVal` | Required |

Returns: no value.

### Choice

`Public Function Choice Of T(generator As RandomGenerator, items As Array Of T) As T`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `generator` | `RandomGenerator` | `ByVal` | Required |
| `items` | `Array Of T` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `T` |

### Sample

`Public Function Sample Of T(generator As RandomGenerator, items As Array Of T, count As Integer) As Array Of T`

Ordered sample of positions without replacement; input is unchanged.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `generator` | `RandomGenerator` | `ByVal` | Required |
| `items` | `Array Of T` | `ByVal` | Required |
| `count` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Array Of T` |
