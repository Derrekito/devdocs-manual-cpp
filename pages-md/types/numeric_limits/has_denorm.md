# std::numeric_limits<T>::has_denorm

```cpp
static const std::float_denorm_style has_denorm;  // (until C++11)
static constexpr std::float_denorm_style has_denorm;  // (since C++11) (deprecated in C++23)
```

The value of `std::numeric_limits<T>::has_denorm` identifies the floating-point
types that support subnormal values.

### Standard specializations

- **/* non-specialized */** — `std::denorm_absent`
- **bool** — `std::denorm_absent`
- **char** — `std::denorm_absent`
- **signed char** — `std::denorm_absent`
- **unsigned char** — `std::denorm_absent`
- **wchar_t** — `std::denorm_absent`
- **char8_t (since C++20)** — `std::denorm_absent`
- **char16_t (since C++11)** — `std::denorm_absent`
- **char32_t (since C++11)** — `std::denorm_absent`
- **short** — `std::denorm_absent`
- **unsigned short** — `std::denorm_absent`
- **int** — `std::denorm_absent`
- **unsigned int** — `std::denorm_absent`
- **long** — `std::denorm_absent`
- **unsigned long** — `std::denorm_absent`
- **long long (since C++11)** — `std::denorm_absent`
- **unsigned long long (since C++11)** — `std::denorm_absent`
- **float** — usually `std::denorm_present`
- **double** — usually `std::denorm_present`
- **long double** — usually `std::denorm_present`

### See also

- **denorm_min [static]** — returns the smallest positive subnormal value of the
  given floating-point type (public static member function)
- **float_denorm_style** — indicates floating-point denormalization modes (enum)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/has_denorm*
