# std::has_single_bit

```cpp
template< class T >
constexpr bool has_single_bit( T x ) noexcept;  // (since C++20)
```

Checks if `x` is an integral power of two.

This overload participates in overload resolution only if `T` is an unsigned
integer type (that is, `unsigned char`, `unsigned short`, `unsigned int`,
`unsigned long`, `unsigned long long`, or an extended unsigned integer type).

### Parameters

- **x** — value of unsigned integer type

### Return value

`true` if `x` is an integral power of two; otherwise `false`.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_int_pow2` | 202002L | (C++20) | Integral power-of-2 operations

### Possible implementation

```cpp
template<std::unsigned_integral T>
    requires !std::same_as<T, bool> && !std::same_as<T, char> &&
             !std::same_as<T, char8_t> && !std::same_as<T, char16_t> &&
             !std::same_as<T, char32_t> && !std::same_as<T, wchar_t>
constexpr bool has_single_bit(T x) noexcept
{
    return x && !(x & (x - 1));
}
```

```cpp
template<std::unsigned_integral T>
    requires !std::same_as<T, bool> && !std::same_as<T, char> &&
             !std::same_as<T, char8_t> && !std::same_as<T, char16_t> &&
             !std::same_as<T, char32_t> && !std::same_as<T, wchar_t>
constexpr bool has_single_bit(T x) noexcept
{
    return std::popcount(x) == 1;
}
```

### Example

```cpp
#include <bit>
#include <bitset>
#include <cmath>
#include <iostream>

int main()
{
    for (auto u = 0u; u != 10; ++u)
    {
        std::cout << "u = " << u << " = " << std::bitset<4>(u);
        if (std::has_single_bit(u)) // `ispow2` before P1956R1
            std::cout << " = 2^" << std::log2(u) << " (is power of two)";
        std::cout << '\n';
    }
}
```

Output:

```text
u = 0 = 0000
u = 1 = 0001 = 2^0 (is power of two)
u = 2 = 0010 = 2^1 (is power of two)
u = 3 = 0011
u = 4 = 0100 = 2^2 (is power of two)
u = 5 = 0101
u = 6 = 0110
u = 7 = 0111
u = 8 = 1000 = 2^3 (is power of two)
u = 9 = 1001
```

### See also

- **popcount (C++20)** — counts the number of `1` bits in an unsigned integer
  (function template)
- **count** — returns the number of bits set to `true` (public member function
  of `std::bitset<N>`)
- **test** — accesses specific bit (public member function of `std::bitset<N>`)

---
*Source: https://en.cppreference.com/w/cpp/numeric/has_single_bit*
