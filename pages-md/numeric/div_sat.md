# std::div_sat

```cpp
template< class T >
constexpr T div_sat( T x, T y ) noexcept;  // (since C++26)
```

Computes the saturating division `x / y`. If `T` is a signed integer type, `x`
is the smallest (most negative) value of `T`, and `y == -1`, returns the
greatest value of `T`; otherwise, returns `x / y`.

`y` must not be `​0​`, otherwise the behavior is undefined.

This overload participates in overload resolution only if `T` is an integer
type, that is: signed char, short, int, long, long long, an extended signed
integer type, or an unsigned version of such types. In particular, `T` must not
be (possibly cv-qualified) bool, char, wchar_t, char8_t, char16_t, and char32_t,
as these types are not intended for arithmetic.

### Parameters

- **x, y** — integer values

### Return value

Saturated `x / y`.

### Exceptions

Throws no exceptions.

### Notes

Unlike the built-in arithmetic operators on integers, the integral promotion
does not apply to the `x` and `y` arguments.

If two arguments of different type are passed, the call fails to compile, i.e.
the behavior relative to template argument deduction is the same as for
`std::min` or `std::max`.

Most modern hardware architectures have efficient support for saturation
arithmetic on SIMD vectors, including SSE2 for x86 and NEON for ARM.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_saturation_arithmetic` | 202311L | (C++26) | Saturation arithmetic

### Possible implementation

See libstdc++ (gcc).

### Example

Can be previewed on Compiler Explorer.

```cpp
#include <climits>
#include <numeric>

static_assert
(""
    && (std::div_sat<int>(6, 3) == 2) // not saturated
    && (std::div_sat<int>(INT_MIN, -1) == INT_MAX) // saturated
    && (std::div_sat<unsigned>(6, 3) == 2) // not saturated
);

int main() {}
```

### See also

- **add_sat (C++26)** — saturating addition operation on two integers (function
  template)
- **sub_sat (C++26)** — saturating subtraction operation on two integers
  (function template)
- **mul_sat (C++26)** — saturating multiplication operation on two integers
  (function template)
- **saturate_cast (C++26)** — returns an integer value clamped to the range of a
  another integer type (function template)
- **clamp (C++17)** — clamps a value between a pair of boundary values (function
  template)
- **in_range (C++20)** — checks if an integer value is in the range of a given
  integer type (function template)
- **min [static]** — returns the smallest finite value of the given type (public
  static member function of `std::numeric_limits<T>`)
- **max [static]** — returns the largest finite value of the given type (public
  static member function of `std::numeric_limits<T>`)

### External links

  1. | A branch-free implementation of saturation arithmetic — Locklessinc.com,
      2012

---
*Source: https://en.cppreference.com/w/cpp/numeric/div_sat*
