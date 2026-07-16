# std::numeric_limits<T>::round_error

```cpp
static T round_error() throw();  // (until C++11)
static constexpr T round_error() noexcept;  // (since C++11)
```

Returns the largest possible rounding error in ULPs (units in the last place) as
defined by ISO 10967, which can vary from `0.5` (rounding to the nearest digit)
to `1.0` (rounding to zero or to infinity). It is only meaningful if
`std::numeric_limits<T>::is_integer == false`.

### Return value

- **/* non-specialized */** — `T()`
- **bool** — `false`
- **char** — `​0​`
- **signed char** — `​0​`
- **unsigned char** — `​0​`
- **wchar_t** — `​0​`
- **char8_t (since C++20)** — `​0​`
- **char16_t (since C++11)** — `​0​`
- **char32_t (since C++11)** — `​0​`
- **short** — `​0​`
- **unsigned short** — `​0​`
- **int** — `​0​`
- **unsigned int** — `​0​`
- **long** — `​0​`
- **unsigned long** — `​0​`
- **long long (since C++11)** — `​0​`
- **unsigned long long (since C++11)** — `​0​`
- **float** — `0.5F`
- **double** — `0.5`
- **long double** — `0.5L`

### See also

- **round_style [static]** — identifies the rounding style used by the type
  (public static member constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/round_error*
