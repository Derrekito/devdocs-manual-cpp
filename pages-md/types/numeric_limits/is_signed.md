# std::numeric_limits<T>::is_signed

```cpp
static const bool is_signed;  // (until C++11)
static constexpr bool is_signed;  // (since C++11)
```

The value of `std::numeric_limits<T>::is_signed` is `true` for all signed
arithmetic types `T` and `false` for the unsigned types. This constant is
meaningful for all specializations.

### Standard specializations

- **/* non-specialized */** — `false`
- **bool** — `false`
- **char** — implementation-defined
- **signed char** — `true`
- **unsigned char** — `false`
- **wchar_t** — implementation-defined
- **char8_t (since C++20)** — `false`
- **char16_t (since C++11)** — `false`
- **char32_t (since C++11)** — `false`
- **short** — `true`
- **unsigned short** — `false`
- **int** — `true`
- **unsigned int** — `false`
- **long** — `true`
- **unsigned long** — `false`
- **long long (since C++11)** — `true`
- **unsigned long long (since C++11)** — `false`
- **float** — `true`
- **double** — `true`
- **long double** — `true`

### Example

```cpp
#include <iostream>
#include <iomanip>
#include <limits>

template<typename T>
struct test
{
    test(const char* name, int w = 15)
    {
        std::cout
            << std::left << std::setw(w)
            << (std::numeric_limits<T>::is_specialized ? name : "non-specialized")
            << " : "
            << (std::numeric_limits<T>::is_signed ? "" : "un") << "signed\n";
    }
};

int main()
{
    test<bool>{"bool"};
    test<char>{"char"};
    test<wchar_t>{"wchar_t"};
    test<char16_t>{"char16_t"};
    test<char32_t>{"char32_t"};
    test<float>{"float"};
    struct delusion{};
    test<delusion>{"delusion"};
    test<decltype(42)>{"decltype(42)"};
}
```

Possible output:

```text
bool            : unsigned
char            : signed
wchar_t         : signed
char16_t        : unsigned
char32_t        : unsigned
float           : signed
non-specialized : unsigned
decltype(42)    : signed
```

### See also

- **is_signed (C++11)** — checks if a type is a signed arithmetic type (class
  template)
- **is_integer [static]** — identifies integer types (public static member
  constant)
- **is_exact [static]** — identifies exact types (public static member constant)
- **is_bounded [static]** — identifies types that represent a finite set of
  values (public static member constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/is_signed*
