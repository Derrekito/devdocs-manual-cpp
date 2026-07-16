# std::numeric_limits<T>::radix

```cpp
static const int radix;  // (until C++11)
static constexpr int radix;  // (since C++11)
```

The value of `std::numeric_limits<T>::radix` is the base of the number system
used in the representation of the type. It is `2` for all binary numeric types,
but it may be, for example, `10` for IEEE 754 decimal floating-point types or
for third-party binary-coded decimal integers. This constant is meaningful for
all specializations.

### Standard specializations

- **/* non-specialized */** — `​0​`
- **bool** — `2`
- **char** — `2`
- **signed char** — `2`
- **unsigned char** — `2`
- **wchar_t** — `2`
- **char8_t (since C++20)** — `2`
- **char16_t (since C++11)** — `2`
- **char32_t (since C++11)** — `2`
- **short** — `2`
- **unsigned short** — `2`
- **int** — `2`
- **unsigned int** — `2`
- **long** — `2`
- **unsigned long** — `2`
- **long long (since C++11)** — `2`
- **unsigned long long (since C++11)** — `2`
- **float** — `FLT_RADIX`
- **double** — `FLT_RADIX`
- **long double** — `FLT_RADIX`

### See also

- **digits [static]** — number of `radix` digits that can be represented without
  change (public static member constant)
- **min_exponent [static]** — one more than the smallest negative power of the
  radix that is a valid normalized floating-point value (public static member
  constant)
- **max_exponent [static]** — one more than the largest integer power of the
  radix that is a valid finite floating-point value (public static member
  constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/radix*
