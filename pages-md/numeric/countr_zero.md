# std::countr_zero

```cpp
template< class T >
constexpr int countr_zero( T x ) noexcept;  // (since C++20)
```

Returns the number of consecutive 0 bits in the value of `x`, starting from the
least significant bit ("right").

This overload participates in overload resolution only if `T` is an unsigned
integer type (that is, `unsigned char`, `unsigned short`, `unsigned int`,
`unsigned long`, `unsigned long long`, or an extended unsigned integer type).

### Parameters

- **x** — value of unsigned integer type

### Return value

The number of consecutive 0 bits in the value of `x`, starting from the least
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
    for (const std::uint8_t i : {0, 0b11111111, 0b00011100, 0b00011101})
        std::cout << "countr_zero( " << std::bitset<8>(i) << " ) = "
                  << std::countr_zero(i) << '\n';
}
```

Output:

```text
countr_zero( 00000000 ) = 8
countr_zero( 11111111 ) = 0
countr_zero( 00011100 ) = 2
countr_zero( 00011101 ) = 0
```

### See also

- **countl_zero (C++20)** — counts the number of consecutive `​0​` bits,
  starting from the most significant bit (function template)
- **countl_one (C++20)** — counts the number of consecutive `1` bits, starting
  from the most significant bit (function template)
- **countr_one (C++20)** — counts the number of consecutive `1` bits, starting
  from the least significant bit (function template)
- **popcount (C++20)** — counts the number of `1` bits in an unsigned integer
  (function template)
- **allanynone** — checks if all, any or none of the bits are set to `true`
  (public member function of `std::bitset<N>`)

---
*Source: https://en.cppreference.com/w/cpp/numeric/countr_zero*
