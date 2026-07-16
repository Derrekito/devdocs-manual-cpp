# std::popcount

```cpp
template< class T >
constexpr int popcount( T x ) noexcept;  // (since C++20)
```

Returns the number of 1 bits in the value of `x`.

This overload participates in overload resolution only if `T` is an unsigned
integer type (that is, `unsigned char`, `unsigned short`, `unsigned int`,
`unsigned long`, `unsigned long long`, or an extended unsigned integer type).

### Parameters

- **x** — value of unsigned integer type

### Return value

The number of 1 bits in the value of `x`.

### Notes

The name `popcount` is a contraction for "population count".

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_bitops` | 201907L | (C++20) | Bit operations

### Example

```cpp
#include <bit>
#include <bitset>
#include <cstdint>
#include <iostream>

static_assert(std::popcount(0xFULL) == 4);

int main()
{
    for (const std::uint8_t x : {0, 0b00011101, 0b11111111})
        std::cout << "popcount( " << std::bitset<8>(x) << " ) = "
                  << std::popcount(x) << '\n';
}
```

Output:

```text
popcount( 00000000 ) = 0
popcount( 00011101 ) = 4
popcount( 11111111 ) = 8
```

### See also

- **countl_zero (C++20)** — counts the number of consecutive `​0​` bits,
  starting from the most significant bit (function template)
- **countl_one (C++20)** — counts the number of consecutive `1` bits, starting
  from the most significant bit (function template)
- **countr_zero (C++20)** — counts the number of consecutive `​0​` bits,
  starting from the least significant bit (function template)
- **countr_one (C++20)** — counts the number of consecutive `1` bits, starting
  from the least significant bit (function template)
- **has_single_bit (C++20)** — checks if a number is an integral power of `2`
  (function template)
- **count** — returns the number of bits set to `true` (public member function
  of `std::bitset<N>`)
- **allanynone** — checks if all, any or none of the bits are set to `true`
  (public member function of `std::bitset<N>`)

---
*Source: https://en.cppreference.com/w/cpp/numeric/popcount*
