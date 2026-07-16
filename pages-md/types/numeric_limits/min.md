# std::numeric_limits<T>::min

```cpp
static T min() throw();  // (until C++11)
static constexpr T min() noexcept;  // (since C++11)
```

Returns the minimum finite value representable by the numeric type `T`.

For floating-point types with denormalization, `min()` returns the minimum
positive normalized value. *Note that this behavior may be unexpected*,
especially when compared to the behavior of `min()` for integral types. To find
the value that has no values less than it, use `lowest()`.(since C++11)

`min()` is only meaningful for bounded types and for unbounded unsigned types,
that is, types that represent an infinite set of negative values have no
meaningful minimum.

### Return value

- **/* non-specialized */** — `T()`
- **bool** — `false`
- **char** — `CHAR_MIN`
- **signed char** — `SCHAR_MIN`
- **unsigned char** — `​0​`
- **wchar_t** — `WCHAR_MIN`
- **char8_t (since C++20)** — `​0​`
- **char16_t (since C++11)** — `​0​`
- **char32_t (since C++11)** — `​0​`
- **short** — `SHRT_MIN`
- **unsigned short** — `​0​`
- **int** — `INT_MIN`
- **unsigned int** — `​0​`
- **long** — `LONG_MIN`
- **unsigned long** — `​0​`
- **long long (since C++11)** — `LLONG_MIN`
- **unsigned long long (since C++11)** — `​0​`
- **float** — `FLT_MIN`
- **double** — `DBL_MIN`
- **long double** — `LDBL_MIN`

### Example

Demonstrates the use with typedef types, and the difference in the sign of the
result between integer and floating-point types:

```cpp
#include <cstddef>
#include <iomanip>
#include <iostream>
#include <limits>

template <typename T>
void print_one(std::string_view type_name)
{
    constexpr T min = std::numeric_limits<T>::min();
    std::cout
        << std::dec << std::defaultfloat << std::setw(14) << type_name
        << " (" << std::setw(2) << sizeof(T) << " bytes): " << min;

    if constexpr (min != 0)
        std::cout << " or " << std::showbase << std::hex << std::hexfloat << min;

    std::cout << '\n';
}

#define SHOW(T) print_one<T>(#T)

int main()
{
    SHOW(bool);
    SHOW(short);
    SHOW(unsigned short);
    SHOW(signed);
    SHOW(unsigned);
    SHOW(std::ptrdiff_t);
    SHOW(std::size_t);
    SHOW(float);
    SHOW(double);
    SHOW(long double);
}
```

Possible output:

```text
          bool ( 1 bytes): 0
         short ( 2 bytes): -32768 or 0x8000
unsigned short ( 2 bytes): 0
        signed ( 4 bytes): -2147483648 or 0x80000000
      unsigned ( 4 bytes): 0
std::ptrdiff_t ( 8 bytes): -9223372036854775808 or 0x8000000000000000
   std::size_t ( 8 bytes): 0
         float ( 4 bytes): 1.17549e-38 or 0x1p-126
        double ( 8 bytes): 2.22507e-308 or 0x1p-1022
   long double (16 bytes): 3.3621e-4932 or 0x8p-16385
```

### See also

- **lowest [static] (C++11)** — returns the lowest finite value of the given
  type (public static member function)
- **denorm_min [static]** — returns the smallest positive subnormal value of the
  given floating-point type (public static member function)
- **max [static]** — returns the largest finite value of the given type (public
  static member function)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/min*
