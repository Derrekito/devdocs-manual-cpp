# std::numeric_limits<T>::round_style

```cpp
static const std::float_round_style round_style;  // (until C++11)
static constexpr std::float_round_style round_style;  // (since C++11)
```

The value of `std::numeric_limits<T>::round_style` identifies the rounding style
used by the floating-point type `T` whenever a value that is not one of the
exactly repesentable values of `T` is stored in an object of that type.

### Standard specializations

- **/* non-specialized */** — `std::round_toward_zero`
- **bool** — `std::round_toward_zero`
- **char** — `std::round_toward_zero`
- **signed char** — `std::round_toward_zero`
- **unsigned char** — `std::round_toward_zero`
- **wchar_t** — `std::round_toward_zero`
- **char8_t (since C++20)** — `std::round_toward_zero`
- **char16_t (since C++11)** — `std::round_toward_zero`
- **char32_t (since C++11)** — `std::round_toward_zero`
- **short** — `std::round_toward_zero`
- **unsigned short** — `std::round_toward_zero`
- **int** — `std::round_toward_zero`
- **unsigned int** — `std::round_toward_zero`
- **long** — `std::round_toward_zero`
- **unsigned long** — `std::round_toward_zero`
- **long long (since C++11)** — `std::round_toward_zero`
- **unsigned long long (since C++11)** — `std::round_toward_zero`
- **float** — usually `std::round_to_nearest`
- **double** — usually `std::round_to_nearest`
- **long double** — usually `std::round_to_nearest`

### Notes

These values are constants, and do not reflect the changes to the rounding made
by `std::fesetround`. The changed values may be obtained from `FLT_ROUNDS` or
`std::fegetround`.

### Example

The decimal value `0.1` cannot be represented by a binary floating-point type.
When stored in an IEEE-754 double, it falls between 0x1.9999999999999*2-4 and
0x1.999999999999a*2-4. Rounding to nearest representable value results in
0x1.999999999999a*2-4.

Similarly, the decimal value `0.3`, which is between 0x1.3333333333333*2-2 and
0x1.3333333333334*2-2 is rounded to nearest and is stored as
0x1.3333333333333*2-2.

```cpp
#include <iostream>
#include <limits>

auto print(std::float_round_style frs)
{
    switch (frs)
    {
        case std::round_indeterminate:
            return "Rounding style cannot be determined";
        case std::round_toward_zero:
            return "Rounding toward zero";
        case std::round_to_nearest:
            return "Rounding toward nearest representable value";
        case std::round_toward_infinity:
            return "Rounding toward positive infinity";
        case std::round_toward_neg_infinity:
            return "Rounding toward negative infinity";
    }
    return "unknown round style";
}

int main()
{
    std::cout << std::hexfloat
              << "The decimal 0.1 is stored in a double as "
              << 0.1 << '\n'
              << "The decimal 0.3 is stored in a double as "
              << 0.3 << '\n'
              << print(std::numeric_limits<double>::round_style) << '\n';
}
```

Output:

```text
The decimal 0.1 is stored in a double as 0x1.999999999999ap-4
The decimal 0.3 is stored in a double as 0x1.3333333333333p-2
Rounding toward nearest representable value
```

### See also

- **float_round_style** — indicates floating-point rounding modes (enum)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/round_style*
