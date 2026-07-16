# std::numeric_limits<T>::has_signaling_NaN

```cpp
static const bool has_signaling_NaN;  // (until C++11)
static constexpr bool has_signaling_NaN;  // (since C++11)
```

The value of `std::numeric_limits<T>::has_signaling_NaN` is `true` for all types
`T` capable of representing the special value "Signaling Not-A-Number". This
constant is meaningful for all floating-point types and is guaranteed to be
`true` if `std::numeric_limits<T>::is_iec559 == true`.

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

- **quiet_NaN [static]** — returns a quiet NaN value of the given floating-point
  type (public static member function)
- **has_infinity [static]** — identifies floating-point types that can represent
  the special value "positive infinity" (public static member constant)
- **has_quiet_NaN [static]** — identifies floating-point types that can
  represent the special value "quiet not-a-number" (NaN) (public static member
  constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/has_signaling_NaN*
