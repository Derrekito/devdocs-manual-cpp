# std::countr_one

```cpp
template< class T >
constexpr int countr_one( T x ) noexcept;  // (since C++20)
```

Returns the number of consecutive 1 bits in the value of `x`, starting from the
least significant bit ("right").

This overload participates in overload resolution only if `T` is an unsigned
integer type (that is, `unsigned char`, `unsigned short`, `unsigned int`,
`unsigned long`, `unsigned long long`, or an extended unsigned integer type).

### Parameters

- **x** — value of unsigned integer type

### Return value

The number of consecutive 1 bits in the value of `x`, starting from the least
significant bit.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_bitops` | 201907L | (C++20) | Bit operations

### Example

```cpp
#include <bit>
#include <bitset>
#include <cstdint>
#include <iostream>

int main()
{
    for (const std::uint8_t i : {0, 0b11111111, 0b11111110, 0b11100011})
        std::cout << "countr_one( " << std::bitset<8>(i) << " ) = "
                  << std::countr_one(i) << '\n';
}
```

Output:

```text
countr_one( 00000000 ) = 0
countr_one( 11111111 ) = 8
countr_one( 11111110 ) = 0
countr_one( 11100011 ) = 2
```

### See also

- **countl_zero (C++20)** — counts the number of consecutive `​0​` bits,
  starting from the most significant bit (function template)
- **countl_one (C++20)** — counts the number of consecutive `1` bits, starting
  from the most significant bit (function template)
- **countr_zero (C++20)** — counts the number of consecutive `​0​` bits,
  starting from the least significant bit (function template)
- **popcount (C++20)** — counts the number of `1` bits in an unsigned integer
  (function template)
- **has_single_bit (C++20)** — checks if a number is an integral power of `2`
  (function template)
- **count** — returns the number of bits set to `true` (public member function
  of `std::bitset<N>`)
- **allanynone** — checks if all, any or none of the bits are set to `true`
  (public member function of `std::bitset<N>`)

---
*Source: https://en.cppreference.com/w/cpp/numeric/countr_one*
