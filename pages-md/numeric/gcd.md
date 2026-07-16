# std::gcd

```cpp
template< class M, class N >
constexpr std::common_type_t<M, N> gcd( M m, N n );  // (since C++17)
```

Computes the greatest common divisor of the integers `m` and `n`.

If either `M` or `N` is not an integer type, or if either is (possibly
cv-qualified) `bool`, the program is ill-formed.

If either `|m|` or `|n|` is not representable as a value of type
`std::common_type_t<M, N>`, the behavior is undefined.

### Parameters

- **m, n** — integer values

### Return value

If both `m` and `n` are zero, returns zero. Otherwise, returns the greatest
common divisor of `|m|` and `|n|`.

### Exceptions

Throws no exceptions.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_gcd_lcm` | 201606L | (C++17) | `std::gcd`, `std::lcm`

### Example

```cpp
#include <numeric>

int main()
{
    constexpr int p{2 * 2 * 3};
    constexpr int q{2 * 3 * 3};
    static_assert(2 * 3 == std::gcd(p, q));

    static_assert(std::gcd( 6,  10) == 2);
    static_assert(std::gcd( 6, -10) == 2);
    static_assert(std::gcd(-6, -10) == 2);

    static_assert(std::gcd( 24, 0) == 24);
    static_assert(std::gcd(-24, 0) == 24);
}
```

### See also

- **lcm (C++17)** — computes the least common multiple of two integers (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/gcd*
