# std::numeric_limits<T>::digits

```cpp
static const int digits;  // (until C++11)
static constexpr int digits;  // (since C++11)
```

The value of `std::numeric_limits<T>::digits` is the number of digits in
base-radix that can be represented by the type `T` without change. For integer
types, this is the number of bits not counting the sign bit and the padding bits
(if any). For floating-point types, this is the digits of the mantissa (for IEC
559/IEEE 754 implementations, this is the number of digits stored for the
mantissa plus one, because the mantissa has an implicit leading 1 and binary
point).

### Standard specializations

- **/* non-specialized */** — `​0​`
- **bool** — `1`
- **char** — `CHAR_BIT - std::numeric_limits<char>::is_signed`
- **signed char** — `CHAR_BIT - 1`
- **unsigned char** — `CHAR_BIT`
- **wchar_t** — `CHAR_BIT * sizeof(wchar_t) -
  std::numeric_limits<wchar_t>::is_signed`
- **char8_t (since C++20)** — `CHAR_BIT`
- **char16_t (since C++11)** — `CHAR_BIT * sizeof(char16_t)`
- **char32_t (since C++11)** — `CHAR_BIT * sizeof(char32_t)`
- **short** — `CHAR_BIT * sizeof(short) - 1`
- **unsigned short** — `CHAR_BIT * sizeof(short)`
- **int** — `CHAR_BIT * sizeof(int) - 1`
- **unsigned int** — `CHAR_BIT * sizeof(int)`
- **long** — `CHAR_BIT * sizeof(long) - 1`
- **unsigned long** — `CHAR_BIT * sizeof(long)`
- **long long (since C++11)** — `CHAR_BIT * sizeof(long long) - 1`
- **unsigned long long (since C++11)** — `CHAR_BIT * sizeof(long long)`
- **float** — `FLT_MANT_DIG`
- **double** — `DBL_MANT_DIG`
- **long double** — `LDBL_MANT_DIG`

### See also

- **radix [static]** — the radix or integer base used by the representation of
  the given type (public static member constant)
- **min_exponent [static]** — one more than the smallest negative power of the
  radix that is a valid normalized floating-point value (public static member
  constant)
- **max_exponent [static]** — one more than the largest integer power of the
  radix that is a valid finite floating-point value (public static member
  constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/digits*
