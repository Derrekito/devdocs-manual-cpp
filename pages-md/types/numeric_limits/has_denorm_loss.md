# std::numeric_limits<T>::has_denorm_loss

```cpp
static const bool has_denorm_loss;  // (until C++11)
static constexpr bool has_denorm_loss;  // (since C++11) (deprecated in C++23)
```

The value of `std::numeric_limits<T>::has_denorm_loss` is `true` for all
floating-point types `T` that detect loss of precision when creating a subnormal
number as denormalization loss rather than as inexact result (see below).

### Standard specializations

- **/* non-specialized */** — `false`
- **bool** — `false`
- **char** — `false`
- **signed char** — `false`
- **unsigned char** — `false`
- **wchar_t** — `false`
- **char8_t (since C++20)** — `false`
- **char16_t (since C++11)** — `false`
- **char32_t (since C++11)** — `false`
- **short** — `false`
- **unsigned short** — `false`
- **int** — `false`
- **unsigned int** — `false`
- **long** — `false`
- **unsigned long** — `false`
- **long long (since C++11)** — `false`
- **unsigned long long (since C++11)** — `false`
- **float** — implementation-defined
- **double** — implementation-defined
- **long double** — implementation-defined

### Notes

Standard-compliant IEEE 754 floating-point implementations of subnormal numbers
are required to detect the loss of accuracy associated with the creation of such
number, if it occurs, and may do so in one of the two distinct ways:

1. Denormalization loss: the delivered result differs from what would have been
   computed were exponent range unbounded.
1. Inexact result: the delivered result differs from what would have been
   computed were both exponent range and precision unbounded.

No implementation of denormalization loss mechanism exists (accuracy loss is
detected after rounding, as inexact result), and this option was removed in the
2008 revision of IEEE Std 754.

libstdc++, libc++, libCstd, and stlport4 define this constant as `false` for all
floating-point types. Microsoft Visual Studio defines it as `true` for all
floating-point types.

As with any floating-point computations, accuracy loss may raise `FE_INEXACT`.

### Example

### See also

- **tinyness_before [static]** — identifies floating-point types that detect
  tinyness before rounding (public static member constant)
- **has_denorm [static]** — identifies the denormalization style used by the
  floating-point type (public static member constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/has_denorm_loss*
