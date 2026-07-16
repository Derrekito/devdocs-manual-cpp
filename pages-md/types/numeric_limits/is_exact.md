# std::numeric_limits<T>::is_exact

```cpp
static const bool is_exact;  // (until C++11)
static constexpr bool is_exact;  // (since C++11)
```

The value of `std::numeric_limits<T>::is_exact` is `true` for all arithmetic
types `T` that use exact representation.

### Standard specializations

- **/* non-specialized */** — `false`
- **bool** — `true`
- **char** — `true`
- **signed char** — `true`
- **unsigned char** — `true`
- **wchar_t** — `true`
- **char8_t (since C++20)** — `true`
- **char16_t (since C++11)** — `true`
- **char32_t (since C++11)** — `true`
- **short** — `true`
- **unsigned short** — `true`
- **int** — `true`
- **unsigned int** — `true`
- **long** — `true`
- **unsigned long** — `true`
- **long long (since C++11)** — `true`
- **unsigned long long (since C++11)** — `true`
- **float** — `false`
- **double** — `false`
- **long double** — `false`

### Notes

While all fundamental types `T` for which `std::numeric_limits<T>::is_exact ==
true` are integer types, a library may define exact types that are not integers,
e.g. a rational arithmetic type representing fractions.

### See also

- **is_integer [static]** — identifies integer types (public static member
  constant)
- **is_signed [static]** — identifies signed types (public static member
  constant)
- **is_bounded [static]** — identifies types that represent a finite set of
  values (public static member constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/is_exact*
