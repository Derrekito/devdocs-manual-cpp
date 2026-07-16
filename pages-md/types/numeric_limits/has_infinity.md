# std::numeric_limits<T>::has_infinity

```cpp
static const bool has_infinity;  // (until C++11)
static constexpr bool has_infinity;  // (since C++11)
```

The value of `std::numeric_limits<T>::has_infinity` is `true` for all types `T`
capable of representing the positive infinity as a distinct special value. This
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

### Example

```cpp
#include <iostream>
#include <limits>

int main()
{
    std::cout << std::boolalpha
              << std::numeric_limits<int>::has_infinity << '\n'
              << std::numeric_limits<long>::has_infinity << '\n'
              << std::numeric_limits<float>::has_infinity << '\n'
              << std::numeric_limits<double>::has_infinity << '\n';
}
```

Possible output:

```text
false
false
true
true
```

### See also

- **infinity [static]** — returns the positive infinity value of the given
  floating-point type (public static member function)
- **has_quiet_NaN [static]** — identifies floating-point types that can
  represent the special value "quiet not-a-number" (NaN) (public static member
  constant)
- **has_signaling_NaN [static]** — identifies floating-point types that can
  represent the special value "signaling not-a-number" (NaN) (public static
  member constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/has_infinity*
