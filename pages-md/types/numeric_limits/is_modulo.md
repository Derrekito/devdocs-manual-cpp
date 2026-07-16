# std::numeric_limits<T>::is_modulo

```cpp
static const bool is_modulo;  // (until C++11)
static constexpr bool is_modulo;  // (since C++11)
```

The value of `std::numeric_limits<T>::is_modulo` is `true` for all arithmetic
types `T` that handle overflows with modulo arithmetic, that is, if the result
of addition, subtraction, multiplication, or division of this type would fall
outside the range `[``min()``,``max()``]`, the value returned by such operation
differs from the expected value by a multiple of `max() - min() + 1`.

`is_modulo` is `false` for signed integer types, unless the implementation
defines signed integer overflow to wrap.

### Standard specializations

- **/* non-specialized */** — `false`
- **bool** — `false`
- **char** — implementation-defined
- **signed char** — implementation-defined
- **unsigned char** — `true`
- **wchar_t** — implementation-defined
- **char8_t (since C++20)** — `true`
- **char16_t (since C++11)** — `true`
- **char32_t (since C++11)** — `true`
- **short** — implementation-defined
- **unsigned short** — `true`
- **int** — implementation-defined
- **unsigned int** — `true`
- **long** — implementation-defined
- **unsigned long** — `true`
- **long long (C++11)** — implementation-defined
- **unsigned long long (C++11)** — `true`
- **float** — `false`
- **double** — `false`
- **long double** — `false`

### Notes

The standard said "On most machines, this is `true` for signed integers." before
the resolution of LWG issue 2422. See GCC PR 22200 for a related discussion.

### Example

Demonstrates the behavior of modulo types:

```cpp
#include <iostream>
#include <type_traits>
#include <limits>

template<class T>
typename std::enable_if<std::numeric_limits<T>::is_modulo>::type
    check_overflow()
{
    std::cout << "max value is " << std::numeric_limits<T>::max() << '\n'
              << "min value is " << std::numeric_limits<T>::min() << '\n'
              << "max value + 1 is " << std::numeric_limits<T>::max()+1 << '\n';
}

int main()
{
    check_overflow<int>();
    std::cout << '\n';
    check_overflow<unsigned long>();
//  check_overflow<float>(); // compile-time error, not a modulo type
}
```

Possible output:

```text
max value is 2147483647
min value is -2147483648
max value + 1 is -2147483648

max value is 18446744073709551615
min value is 0
max value + 1 is 0
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 612 | C++98 | the definition of "handle overflows with modulo arithmetic"
      was poor[1] | provided a better definition
  LWG 2422 | C++98 | `is_modulo` was required to be `true` for signed integer
      types on most machines | required to be `false` for signed integer types
      unless signed integer overflow is defined to wrap

1. The definition is "adding two positive numbers can have a result that wraps
   around to a third number that is less". It has the following problems: It
   does not define the wrapped value. It does not state whether result is
   repeatable. It does not require that doing addition, subtraction and other
   operations on all values have defined behavior.

### See also

- **is_integer [static]** — identifies integer types (public static member
  constant)
- **is_iec559 [static]** — identifies the IEC 559/IEEE 754 floating-point types
  (public static member constant)
- **is_exact [static]** — identifies exact types (public static member constant)

---
*Source: https://en.cppreference.com/w/cpp/types/numeric_limits/is_modulo*
