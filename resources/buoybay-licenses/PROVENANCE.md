# Algorithm provenance

MT19937 by Makoto Matsumoto and Takuji Nishimura, with the January 2002
single-word initialization recurrence, as documented by the authors:
https://www.math.sci.hiroshima-u.ac.jp/m-mat/MT/MT2002/emt19937ar.html
Reference C source:
https://www.math.sci.hiroshima-u.ac.jp/m-mat/MT/MT2002/CODES/mt19937ar.c

The algorithm parameters, initialization, tempering, and 53-bit conversion follow
that established reference. The reference source's copyright and BSD terms are
retained in UPSTREAM-LICENSE.txt. The Buoy implementation replaces unsigned C
arithmetic/shifts with bounded nonnegative signed Integer operations. The largest
seed multiplication plus index is below 2^63; left-shift equivalents during
tempering are below 2^47. There is no reliance on overflow, UInt64 casts, or signed
right shifts. MT19937 is used instead of xoshiro256** because this representation
avoids needing assumptions about unsigned wrapping/shift code generation.

Inclusive signed-bound sampling uses two base-2^32 limbs for rank subtraction and
addition and bit-length rejection. It never computes an overflowing signed span.
This bounded sampling code and the array snapshot/state wrappers are local work.
Shuffle is Fisher-Yates; sample uses a partial forward Fisher-Yates copy; the
normal transform is Box-Muller and exponential uses the inverse CDF. No other
package source or native library is bundled.

Raw words are checked across multiple twists against both a masked Python
reference and CPython's standard-library MT engine (injected identical state,
not Python's different seed mapping). The 5489 first word is also a fixed known
answer. This is reproducibility evidence on the tested Linux host, not a claim of
formal conformance, stream independence, or non-Linux native behavior.
