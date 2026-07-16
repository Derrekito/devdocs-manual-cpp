# std::numeric_limits<T>::is_iec559

```cpp
static const bool is_iec559;  // (until C++11)
static constexpr bool is_iec559;  // (since C++11)
```

The value of `std::numeric_limits<T>::is_iec559` is `true` for all
floating-point types `T` which fulfill the requirements of IEC 559 (IEEE 754)
standard. If `std::numeric_limits<T>::is_iec559` is `true`, then
`std::numeric_limits<T>::has_infinity`, `std::numeric_limits<T>::has_quiet_NaN`,
and `std::numeric_limits<T>::has_signaling_NaN` are also `true`.

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
- **float** — usually `true`
- **double** — usually `true`
- **long double** — usually `true`

### See also

- **has_infinity [static]** — identifies floating-point types that can represent
  the special value "positive infinity" (public static member constant)
- **has_quiet_NaN [static]** — identifies floating-point types that can
  represent the special value "quiet not-a-number" (NaN) (public static member
  constant)
- **has_signaling_NaN [static]** — identifies floating-point types that can
  represent the special value "signaling not-a-number" (NaN) (public static
  member constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/is_iec559*
