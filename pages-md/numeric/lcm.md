# std::lcm

```cpp
template< class M, class N >
constexpr std::common_type_t<M, N> lcm( M m, N n );  // (since C++17)
```

Computes the least common multiple of the integers `m` and `n`.

If either `M` or `N` is not an integer type, or if either is (possibly
cv-qualified) `bool`, the program is ill-formed.

The behavior is undefined if `|m|`, `|n|`, or the least common multiple of `|m|`
and `|n|` is not representable as a value of type `std::common_type_t<M, N>`.

### Parameters

- **m, n** — integer values

### Return value

If either `m` or `n` is zero, returns zero. Otherwise, returns the least common
multiple of `|m|` and `|n|`.

### Exceptions

Throws no exceptions.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_gcd_lcm` | 201606L | (C++17) | `std::gcd`, `std::lcm`

### Example

```cpp
#include <iostream>
#include <numeric>

#define OUT(...) std::cout << #__VA_ARGS__ << " = " << __VA_ARGS__ << '\n'

constexpr auto lcm(auto x, auto... xs)
{
    return ((x = std::lcm(x, xs)), ...);
}

int main()
{
    constexpr int p{2 * 2 * 3};
    constexpr int q{2 * 3 * 3};
    static_assert(2 * 2 * 3 * 3 == std::lcm(p, q));
    static_assert(225 == std::lcm(45, 75));

    static_assert(std::lcm( 6,  10) == 30);
    static_assert(std::lcm( 6, -10) == 30);
    static_assert(std::lcm(-6, -10) == 30);

    static_assert(std::lcm( 24, 0) == 0);
    static_assert(std::lcm(-24, 0) == 0);

    OUT(lcm(2 * 3, 3 * 4, 4 * 5));
    OUT(lcm(2 * 3 * 4, 3 * 4 * 5, 4 * 5 * 6));
    OUT(lcm(2 * 3 * 4, 3 * 4 * 5, 4 * 5 * 6, 5 * 6 * 7));
}
```

Output:

```text
lcm(2 * 3, 3 * 4, 4 * 5) = 60
lcm(2 * 3 * 4, 3 * 4 * 5, 4 * 5 * 6) = 120
lcm(2 * 3 * 4, 3 * 4 * 5, 4 * 5 * 6, 5 * 6 * 7) = 840
```

### See also

- **gcd (C++17)** — computes the greatest common divisor of two integers
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/lcm*
