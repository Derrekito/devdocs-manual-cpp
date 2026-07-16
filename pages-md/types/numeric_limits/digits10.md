# std::numeric_limits<T>::digits10

```cpp
static const int digits10;  // (until C++11)
static constexpr int digits10;  // (since C++11)
```

The value of `std::numeric_limits<T>::digits10` is the number of base-10 digits
that can be represented by the type `T` without change, that is, any number with
this many significant decimal digits can be converted to a value of type `T` and
back to decimal form, without change due to rounding or overflow. For base-radix
types, it is the value of `digits()` (`digits - 1` for floating-point types)
multiplied by \(\small \log_{10}{radix}\)log10(radix) and rounded down.

### Standard specializations

- **/* non-specialized */** — `​0​`
- **bool** — `​0​`
- **char** — `std::numeric_limits<char>::digits * std::log10(2)`
- **signed char** — `std::numeric_limits<signed char>::digits * std::log10(2)`
- **unsigned char** — `std::numeric_limits<unsigned char>::digits *
  std::log10(2)`
- **wchar_t** — `std::numeric_limits<wchar_t>::digits * std::log10(2)`
- **char8_t (since C++20)** — `std::numeric_limits<char8_t>::digits *
  std::log10(2)`
- **char16_t (since C++11)** — `std::numeric_limits<char16_t>::digits *
  std::log10(2)`
- **char32_t (since C++11)** — `std::numeric_limits<char32_t>::digits *
  std::log10(2)`
- **short** — `std::numeric_limits<short>::digits * std::log10(2)`
- **unsigned short** — `std::numeric_limits<unsigned short>::digits *
  std::log10(2)`
- **int** — `std::numeric_limits<int>::digits * std::log10(2)`
- **unsigned int** — `std::numeric_limits<unsigned int>::digits * std::log10(2)`
- **long** — `std::numeric_limits<long>::digits * std::log10(2)`
- **unsigned long** — `std::numeric_limits<unsigned long>::digits *
  std::log10(2)`
- **long long (since C++11)** — `std::numeric_limits<long long>::digits *
  std::log10(2)`
- **unsigned long long (since C++11)** — `std::numeric_limits<unsigned long
  long>::digits * std::log10(2)`
- **float** — `FLT_DIG` (`6` for IEEE float)
- **double** — `DBL_DIG` (`15` for IEEE double)
- **long double** — `LDBL_DIG` (`18` for 80-bit Intel long double; `33` for IEEE
  quadruple)

### Example

An 8-bit binary type can represent any two-digit decimal number exactly, but
3-digit decimal numbers 256..999 cannot be represented. The value of `digits10`
for an 8-bit type is 2 (`8 * std::log10(2)` is 2.41)

The standard 32-bit IEEE 754 floating-point type has a 24 bit fractional part
(23 bits written, one implied), which may suggest that it can represent 7 digit
decimals (`24 * std::log10(2)` is 7.22), but relative rounding errors are
non-uniform and some floating-point values with 7 decimal digits do not survive
conversion to 32-bit float and back: the smallest positive example is
`8.589973e9`, which becomes `8.589974e9` after the roundtrip. These rounding
errors cannot exceed one bit in the representation, and `digits10` is calculated
as `(24 - 1) * std::log10(2)`, which is 6.92. Rounding down results in the value
6.

Likewise, the 16-digit string `9007199254740993` does not survive
text->double->text roundtrip, becoming `9007199254740992`: the 64-bit IEEE 754
type double guarantees this roundtrip only for 15 decimal digits.

### See also

- **max_digits10 [static] (C++11)** — number of decimal digits necessary to
  differentiate all values of this type (public static member constant)
- **radix [static]** — the radix or integer base used by the representation of
  the given type (public static member constant)
- **digits [static]** — number of `radix` digits that can be represented without
  change (public static member constant)
- **min_exponent [static]** — one more than the smallest negative power of the
  radix that is a valid normalized floating-point value (public static member
  constant)
- **max_exponent [static]** — one more than the largest integer power of the
  radix that is a valid finite floating-point value (public static member
  constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/digits10*
