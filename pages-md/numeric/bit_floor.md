# std::bit_floor

```cpp
template< class T >
constexpr T bit_floor( T x ) noexcept;  // (since C++20)
```

If `x` is not zero, calculates the largest integral power of two that is not
greater than `x`. If `x` is zero, returns zero.

This overload participates in overload resolution only if `T` is an unsigned
integer type (that is, `unsigned char`, `unsigned short`, `unsigned int`,
`unsigned long`, `unsigned long long`, or an extended unsigned integer type).

### Parameters

- **x** — unsigned integer value

### Return value

Zero if `x` is zero; otherwise, the largest integral power of two that is not
greater than `x`.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_int_pow2` | 202002L | (C++20) | Integral power-of-2 operations

### Possible implementation

```cpp
template<std::unsigned_integral T>
    requires !std::same_as<T, bool> && !std::same_as<T, char> &&
             !std::same_as<T, char8_t> && !std::same_as<T, char16_t> &&
             !std::same_as<T, char32_t> && !std::same_as<T, wchar_t>
constexpr T bit_floor(T x) noexcept
{
    if (x != 0)
        return T{1} << (std::bit_width(x) - 1);
    return 0;
}
```

### Example

```cpp
#include <bit>
#include <bitset>
#include <iostream>

int main()
{
    using bin = std::bitset<8>;

    for (unsigned x = 0; x != 10; ++x)
    {
        auto const z = std::bit_floor(x); // `floor2` before P1956R1
        std::cout << "bit_floor( " << bin(x) << " ) = " << bin(z) << '\n';
    }
}
```

Output:

```text
bit_floor( 00000000 ) = 00000000
bit_floor( 00000001 ) = 00000001
bit_floor( 00000010 ) = 00000010
bit_floor( 00000011 ) = 00000010
bit_floor( 00000100 ) = 00000100
bit_floor( 00000101 ) = 00000100
bit_floor( 00000110 ) = 00000100
bit_floor( 00000111 ) = 00000100
bit_floor( 00001000 ) = 00001000
bit_floor( 00001001 ) = 00001000
```

### See also

- **bit_ceil (C++20)** — finds the smallest integral power of two not less than
  the given value (function template)
- **rotr (C++20)** — computes the result of bitwise right-rotation (function
  template)
- **bit_width (C++20)** — finds the smallest number of bits needed to represent
  the given value (function template)
- **has_single_bit (C++20)** — checks if a number is an integral power of `2`
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/bit_floor*
