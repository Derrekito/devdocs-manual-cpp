# std::rotl

```cpp
template< class T >
[[nodiscard]] constexpr T rotl( T x, int s ) noexcept;  // (since C++20)
```

Computes the result of bitwise left-rotating the value of `x` by `s` positions.
This operation is also known as a left circular shift.

Formally, let `N` be `std::numeric_limits<T>::digits`, `r` be `s % N`.

- If `r` is 0, returns `x`;
- if `r` is positive, returns `(x << r) | (x >> (N - r))`;
- if `r` is negative, returns `std::rotr(x, -r)`.

This overload participates in overload resolution only if `T` is an unsigned
integer type (that is, `unsigned char`, `unsigned short`, `unsigned int`,
`unsigned long`, `unsigned long long`, or an extended unsigned integer type).

### Parameters

- **x** — value of unsigned integer type
- **s** — number of positions to shift

### Return value

The result of bitwise left-rotating `x` by `s` positions.

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
    const std::uint8_t i = 0b00011101;
    std::cout << "i          = " << std::bitset<8>(i) << '\n';
    std::cout << "rotl(i,0)  = " << std::bitset<8>(std::rotl(i, 0)) << '\n';
    std::cout << "rotl(i,1)  = " << std::bitset<8>(std::rotl(i, 1)) << '\n';
    std::cout << "rotl(i,4)  = " << std::bitset<8>(std::rotl(i, 4)) << '\n';
    std::cout << "rotl(i,9)  = " << std::bitset<8>(std::rotl(i, 9)) << '\n';
    std::cout << "rotl(i,-1) = " << std::bitset<8>(std::rotl(i, -1)) << '\n';
}
```

Output:

```text
i          = 00011101
rotl(i,0)  = 00011101
rotl(i,1)  = 00111010
rotl(i,4)  = 11010001
rotl(i,9)  = 00111010
rotl(i,-1) = 10001110
```

### See also

- **rotr (C++20)** — computes the result of bitwise right-rotation (function
  template)
- **operator<<=operator>>=operator<<operator>>** — performs binary shift left
  and shift right (public member function of `std::bitset<N>`)

---
*Source: https://en.cppreference.com/w/cpp/numeric/rotl*
