# std::numeric_limits<T>::is_specialized

```cpp
static const bool is_specialized;  // (until C++11)
static constexpr bool is_specialized;  // (since C++11)
```

The value of `std::numeric_limits<T>::is_specialized` is `true` for all `T` for
which there exists a specialization of `std::numeric_limits`.

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
- **float** — `true`
- **double** — `true`
- **long double** — `true`

### Example

```cpp
#include <iostream>
#include <limits>
#include <type_traits>

int main()
{
    enum E{};

    std::cout << std::boolalpha
              << std::numeric_limits<bool>::is_specialized << '\n'
              << std::numeric_limits<long long>::is_specialized << '\n'
              << std::numeric_limits<std::true_type>::is_specialized << '\n'
              << std::numeric_limits<E>::is_specialized << '\n';
}
```

Possible output:

```text
true
true
false
false
```

### See also

- **is_integer [static]** — identifies integer types (public static member
  constant)
- **is_iec559 [static]** — identifies the IEC 559/IEEE 754 floating-point types
  (public static member constant)
- **is_exact [static]** — identifies exact types (public static member constant)
- **is_bounded [static]** — identifies types that represent a finite set of
  values (public static member constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/is_specialized*
