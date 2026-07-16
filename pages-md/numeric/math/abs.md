# std::abs, std::labs, std::llabs, std::imaxabs

```cpp
int       abs( int num );  // (1) (constexpr since C++23)
long      abs( long num );  // (2) (constexpr since C++23)
long long abs( long long num );  // (3) (since C++11) (constexpr since C++23)
long       labs( long num );  // (4) (constexpr since C++23)
long long llabs( long long num );  // (5) (since C++11) (constexpr since C++23)
std::intmax_t abs( std::intmax_t num );  // (6) (since C++11) (constexpr since C++23)
std::intmax_t imaxabs( std::intmax_t num );  // (7) (since C++11) (constexpr since C++23)
```

Computes the absolute value of the integer number `num`. The behavior is
undefined if the result cannot be represented by the return type.

If `std::abs` is called with an unsigned integral argument that cannot be
converted to int by integral promotion, the program is ill-formed.

Overload (6) of `std::abs` for `std::intmax_t` is provided in `<cinttypes>` if
and only if `std::intmax_t` is an extended integer type.
*(since C++11)*

### Parameters

- **num** — integer value

### Return value

The absolute value of `num` (i.e. `|num|`), if it is representable.

### Notes

In 2's complement systems, the absolute value of the most-negative value is out
of range, e.g. for 32-bit 2's complement type int, `INT_MIN` is `-2147483648`,
but the would-be result `2147483648` is greater than `INT_MAX`, which is
`2147483647`.

### Example

```cpp
#include <climits>
#include <cstdlib>
#include <iostream>

int main()
{
    std::cout << std::showpos
              << "abs(+3) = " << std::abs(3) << '\n'
              << "abs(-3) = " << std::abs(-3) << '\n';

//  std::cout << std::abs(INT_MIN); // undefined behavior on 2's complement systems
}
```

Output:

```text
abs(+3) = +3
abs(-3) = +3
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2192 | C++98 | overloads of `std::abs` were inconsistently declared in two
      headers | declared these overloads in both headers

### See also

- **abs(float)fabsfabsffabsl (C++11)(C++11)** — absolute value of a floating
  point value (\(\small{|x|}\)|x|) (function)
- **abs(std::complex)** — returns the magnitude of a complex number (function
  template)
- **abs(std::valarray)** — applies the function `abs` to each element of
  valarray (function template)

**C documentation for `abs, labs, llabs`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/abs*
