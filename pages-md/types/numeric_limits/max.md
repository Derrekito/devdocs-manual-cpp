# std::numeric_limits<T>::max

```cpp
static T max() throw();  // (until C++11)
static constexpr T max() noexcept;  // (since C++11)
```

Returns the maximum finite value representable by the numeric type `T`.
Meaningful for all bounded types.

### Return value

- **/* non-specialized */** — `T()`
- **bool** — `true`
- **char** — `CHAR_MAX`
- **signed char** — `SCHAR_MAX`
- **unsigned char** — `UCHAR_MAX`
- **wchar_t** — `WCHAR_MAX`
- **char8_t (since C++20)** — `UCHAR_MAX`
- **char16_t (since C++11)** — `UINT_LEAST16_MAX`
- **char32_t (since C++11)** — `UINT_LEAST32_MAX`
- **short** — `SHRT_MAX`
- **unsigned short** — `USHRT_MAX`
- **int** — `INT_MAX`
- **unsigned int** — `UINT_MAX`
- **long** — `LONG_MAX`
- **unsigned long** — `ULONG_MAX`
- **long long (since C++11)** — `LLONG_MAX`
- **unsigned long long (since C++11)** — `ULLONG_MAX`
- **float** — `FLT_MAX`
- **double** — `DBL_MAX`
- **long double** — `LDBL_MAX`

### Example

Demonstrates the use of `max()` with some fundamental types and some standard
library typedefs (the output is system-specific):

```cpp
#include <cstddef>
#include <iostream>
#include <limits>
#include <string_view>
#include <type_traits>

template<typename T>
void print_max_twice(std::string_view type)
{
    constexpr T max_value {std::numeric_limits<T>::max()};

    std::cout << type << ": ";
    if constexpr (std::is_floating_point_v<T>)
        std::cout << std::defaultfloat << max_value << " or "
                  << std::hexfloat << max_value << '\n';
    else
        std::cout << std::dec << static_cast<unsigned long long>(max_value) << " or "
                  << std::hex << static_cast<unsigned long long>(max_value) << '\n';
}

int main()
{
    std::cout << std::showbase;

    print_max_twice<bool>("bool");
    print_max_twice<short>("short");
    print_max_twice<int>("int");
    print_max_twice<std::streamsize>("streamsize");
    print_max_twice<std::size_t>("size_t");
    print_max_twice<char>("char");
    print_max_twice<char16_t>("char16_t");
    print_max_twice<wchar_t>("wchar_t");
    print_max_twice<float>("float");
    print_max_twice<double>("double");
    print_max_twice<long double>("long double");
}
```

Possible output:

```text
bool: 1 or 0x1
short: 32767 or 0x7fff
int: 2147483647 or 0x7fffffff
streamsize: 9223372036854775807 or 0x7fffffffffffffff
size_t: 18446744073709551615 or 0xffffffffffffffff
char: 127 or 0x7f
char16_t: 65535 or 0xffff
wchar_t: 2147483647 or 0x7fffffff
float: 3.40282e+38 or 0x1.fffffep+127
double: 1.79769e+308 or 0x1.fffffffffffffp+1023
long double: 1.18973e+4932 or 0xf.fffffffffffffffp+16380
```

### See also

- **lowest [static] (C++11)** — returns the lowest finite value of the given
  type (public static member function)
- **min [static]** — returns the smallest finite value of the given type (public
  static member function)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/max*
