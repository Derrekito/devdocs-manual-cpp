# std::numeric_limits

Compile-time facts about a numeric type: its largest value, whether it
has infinity, how many decimal digits it can represent, and more.
Every property is a `static` member of `std::numeric_limits<T>`, so
`std::numeric_limits<int>::max()` is a constant, not a function call.

**The classic trap:** for floating-point types, `min()` is the
smallest *positive* representable value, not the most negative one —
`std::numeric_limits<float>::min()` is about `1.18e-38`, a tiny
positive number. The true bottom of the range is `lowest()`
(C++11): `std::numeric_limits<float>::lowest()` is about `-3.4e+38`.
For integer types the two agree, which is exactly what makes the
float case surprising the first time you hit it.

```cpp skip
std::numeric_limits<int>::max();       // largest int
std::numeric_limits<int>::min();       // smallest int (== lowest() for integers)
std::numeric_limits<float>::min();     // smallest POSITIVE float, NOT the bottom
std::numeric_limits<float>::lowest();  // true bottom of the range   (since C++11)
std::numeric_limits<float>::epsilon(); // gap above 1.0
```

### What you provide

- **T** — the type to query, one of the built-in arithmetic types.
  The library specializes `numeric_limits` for all of them (`bool`,
  the char types, the integer types, `float`/`double`/`long double`);
  cv-qualified `T` gives the same values as unqualified `T`. Type
  aliases such as `std::size_t` work too. Non-arithmetic types (e.g.
  `std::complex<T>`) have no specialization.

### Guarantees and costs

- Every member is a compile-time constant or `constexpr` function —
  no runtime cost, usable in constant expressions.
- `is_specialized` is `false` for any type without a real
  specialization; querying an unspecialized type gives all
  zero/false/default values rather than an error, so check
  `is_specialized` before trusting the rest for an unfamiliar `T`.
- Library authors may specialize `numeric_limits` for their own
  arithmetic-like types (e.g. a 128-bit int or a half-float) so
  generic code can query them the same way.

### Gotchas

- `min()` vs `lowest()` — see above. Reaching for `min()` when you
  meant "most negative" is the most common misuse of this class.
- `epsilon()` is the gap above `1.0`, not a general-purpose "close
  enough" tolerance for arbitrary-magnitude floating-point
  comparisons.
- Comparing against `max()`/`lowest()` to detect overflow only works
  if the arithmetic that produced the value didn't already overflow
  (which is UB for signed integers) — check before the operation, not
  just the result.

### Example

```cpp
#include <iostream>
#include <limits>

int main()
{
    std::cout << "int:   min=" << std::numeric_limits<int>::min()
              << " lowest=" << std::numeric_limits<int>::lowest()
              << " max=" << std::numeric_limits<int>::max() << '\n';

    std::cout << "float: min=" << std::numeric_limits<float>::min()
              << " lowest=" << std::numeric_limits<float>::lowest()
              << " max=" << std::numeric_limits<float>::max() << '\n';
}
```

```text
int:   min=-2147483648 lowest=-2147483648 max=2147483647
float: min=1.17549e-38 lowest=-3.40282e+38 max=3.40282e+38
```

### Reference

```cpp skip
template< class T > class numeric_limits;

template<> class numeric_limits<bool>;
template<> class numeric_limits<char>;
template<> class numeric_limits<signed char>;
template<> class numeric_limits<unsigned char>;
template<> class numeric_limits<wchar_t>;
template<> class numeric_limits<char8_t>;              // (since C++20)
template<> class numeric_limits<char16_t>;              // (since C++11)
template<> class numeric_limits<char32_t>;              // (since C++11)
template<> class numeric_limits<short>;
template<> class numeric_limits<unsigned short>;
template<> class numeric_limits<int>;
template<> class numeric_limits<unsigned int>;
template<> class numeric_limits<long>;
template<> class numeric_limits<unsigned long>;
template<> class numeric_limits<long long>;              // (since C++11)
template<> class numeric_limits<unsigned long long>;      // (since C++11)
template<> class numeric_limits<float>;
template<> class numeric_limits<double>;
template<> class numeric_limits<long double>;
```

Member constants (all `public static`, condensed):

- **is_specialized** — true if this `T` has a real specialization
- **is_signed / is_integer / is_exact** — basic classification of `T`
- **has_infinity / has_quiet_NaN / has_signaling_NaN** — can `T`
  represent those special values
- **has_denorm / has_denorm_loss** — subnormal-value support and
  whether their precision loss is reported distinctly
- **round_style / is_iec559** — rounding mode; whether `T` is IEEE 754
- **is_bounded / is_modulo** — finite range; wraps on overflow
- **digits / digits10 / max_digits10 (C++11)** — representable digits
  in `radix`, in decimal, and needed to round-trip a value
- **radix** — base of the representation (usually `2`)
- **min_exponent / min_exponent10 / max_exponent / max_exponent10** —
  bounds on the normalized exponent range
- **traps / tinyness_before** — trapping on arithmetic; rounding
  detail for subnormals

Member functions (all `public static`):

- **min()** — smallest finite value; smallest *positive* value for
  floating-point types
- **lowest()** (C++11) — most negative finite value
- **max()** — largest finite value
- **epsilon()** — gap between `1.0` and the next representable value
- **round_error()** — maximum rounding error
- **infinity()** — positive infinity, if `has_infinity`
- **quiet_NaN() / signaling_NaN()** — NaN values, if supported
- **denorm_min()** — smallest positive subnormal value

Helper classes: **float_round_style** and **float_denorm_style**
(enums describing rounding/denormalization modes). The C `<climits>`
/ `<cfloat>` macros (`INT_MAX`, `FLT_MIN`, ...) mirror these members
for the corresponding built-in type.

### See also

- **Fixed width integer types** — `int32_t` and friends
- **numeric_limits::min** — the smallest-positive-value trap in detail
- **numeric_limits::lowest** — the true bottom of the range

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits*
