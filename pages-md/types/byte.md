# std::byte

```cpp
enum class byte : unsigned char {};  // (since C++17)
```

`std::byte` is a distinct type that implements the concept of byte as specified
in the C++ language definition.

Like `unsigned char`, it can be used to access raw memory occupied by other
objects (object representation), but unlike `unsigned char`, it is not a
character type and is not an arithmetic type. `std::byte` models a mere
collection of bits, supporting only bitwise and comparison operations.

### Non-member functions

## std::to_integer

```cpp
template <class IntegerType>
 constexpr IntegerType to_integer( std::byte b ) noexcept;  // (since C++17)
```

Equivalent to: `return IntegerType(b);` This overload participates in overload
resolution only if `std::is_integral_v<IntegerType>` is true.

## std::operator<<=,operator>>=

```cpp
template <class IntegerType>
 constexpr std::byte& operator<<=( std::byte& b, IntegerType shift ) noexcept;  // (1) (since C++17)
template <class IntegerType>
 constexpr std::byte& operator>>=( std::byte& b, IntegerType shift ) noexcept;  // (2) (since C++17)
```

1) Equivalent to: `return b = b << shift;` This overload participates in
   overload resolution only if `std::is_integral_v<IntegerType>` is `true`.

2) Equivalent to: `return b = b >> shift;` This overload participates in
   overload resolution only if `std::is_integral_v<IntegerType>` is true.

## std::operator<<,operator>>

```cpp
template <class IntegerType>
 constexpr std::byte operator<<( std::byte b, IntegerType shift ) noexcept;  // (1) (since C++17)
template <class IntegerType>
 constexpr std::byte operator>>( std::byte b, IntegerType shift ) noexcept;  // (2) (since C++17)
```

1) Equivalent to: `return std::byte(static_cast<unsigned int>(b) << shift);`
   This overload participates in overload resolution only if
   `std::is_integral_v<IntegerType>` is `true`.

2) Equivalent to: `return std::byte(static_cast<unsigned int>(b) >> shift);`
   This overload participates in overload resolution only if
   `std::is_integral_v<IntegerType>` is true.

## std::operator|=,operator&=,operator^=

```cpp
constexpr std::byte& operator|=( std::byte& l, std::byte r ) noexcept;  // (1) (since C++17)
constexpr std::byte& operator&=( std::byte& l, std::byte r ) noexcept;  // (2) (since C++17)
constexpr std::byte& operator^=( std::byte& l, std::byte r ) noexcept;  // (3) (since C++17)
```

1) Equivalent to: `return l = l | r;`.

2) Equivalent to: `return l = l & r;`.

3) Equivalent to: `return l = l ^ r;`.

## std::operator|,operator&,operator^,operator~

```cpp
constexpr std::byte operator|( std::byte l, std::byte r ) noexcept;  // (1) (since C++17)
constexpr std::byte operator&( std::byte l, std::byte r ) noexcept;  // (2) (since C++17)
constexpr std::byte operator^( std::byte l, std::byte r ) noexcept;  // (3) (since C++17)
constexpr std::byte operator~( std::byte b ) noexcept;  // (4) (since C++17)
```

1) Equivalent to: `return std::byte(static_cast<unsigned int>(l) |
   static_cast<unsigned int>(r));`.

2) Equivalent to: `return std::byte(static_cast<unsigned int>(l) &
   static_cast<unsigned int>(r));`.

3) Equivalent to: `return std::byte(static_cast<unsigned int>(l) ^
   static_cast<unsigned int>(r));`.

4) Equivalent to: `return std::byte(~static_cast<unsigned int>(b));`

### Notes

A numeric value `n` can be converted to a byte value using `std::byte{n}`, due
to C++17 relaxed enum class initialization rules.

A byte can be converted to a numeric value (such as to produce an integer hash
of an object) the usual way with an explicit conversion or alternatively with
`std::to_integer`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_byte` | 201603L | (C++17) | `std::byte`

### Example

```cpp
#include <bitset>
#include <cstddef>
#include <iostream>

std::ostream& operator<<(std::ostream& os, std::byte b)
{
    return os << std::bitset<8>(std::to_integer<int>(b));
}

int main()
{
    std::byte b{42};
    std::cout << "1. " << b << '\n';

    // b *= 2 compilation error
    b <<= 1;
    std::cout << "2. " << b << '\n';

    b >>= 1;
    std::cout << "3. " << b << '\n';

    std::cout << "4. " << (b << 1) << '\n';
    std::cout << "5. " << (b >> 1) << '\n';

    b |= std::byte{0b11110000};
    std::cout << "6. " << b << '\n';

    b &= std::byte{0b11110000};
    std::cout << "7. " << b << '\n';

    b ^= std::byte{0b11111111};
    std::cout << "8. " << b << '\n';
}
```

Output:

```text
1. 00101010
2. 01010100
3. 00101010
4. 01010100
5. 00010101
6. 11111010
7. 11110000
8. 00001111
```

---
*Source: https://en.cppreference.com/w/cpp/types/byte*
