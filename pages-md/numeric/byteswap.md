# std::byteswap

```cpp
template< class T >
constexpr T byteswap( T n ) noexcept;  // (since C++23)
```

Reverses the bytes in the given integer value `n`.

`std::byteswap` participates in overload resolution only if `T` satisfies
`integral`, i.e., `T` is an integer type. The program is ill-formed if `T` has
padding bits.

### Parameters

- **n** — integer value

### Return value

An integer value of type `T` whose object representation comprises the bytes of
that of `n` in reversed order.

### Notes

This function is useful for processing data of different endianness.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_byteswap` | 202110L | (C++23) | `std::byteswap`

### Possible implementation

```cpp
template<std::integral T>
constexpr T byteswap(T value) noexcept
{
    static_assert(std::has_unique_object_representations_v<T>,
                  "T may not have padding bits");
    auto value_representation = std::bit_cast<std::array<std::byte, sizeof(T)>>(value);
    std::ranges::reverse(value_representation);
    return std::bit_cast<T>(value_representation);
}
```

### Example

```cpp
#include <bit>
#include <concepts>
#include <cstdint>
#include <iomanip>
#include <iostream>

template<std::integral T>
void dump(T v, char term = '\n')
{
    std::cout << std::hex << std::uppercase << std::setfill('0')
              << std::setw(sizeof(T) * 2) << v << " : ";
    for (std::size_t i{}; i != sizeof(T); ++i, v >>= 8)
        std::cout << std::setw(2) << static_cast<unsigned>(T(0xFF) & v) << ' ';
    std::cout << std::dec << term;
}

int main()
{
    static_assert(std::byteswap('a') == 'a');

    std::cout << "byteswap for U16:\n";
    constexpr auto x = std::uint16_t(0xCAFE);
    dump(x);
    dump(std::byteswap(x));

    std::cout << "\nbyteswap for U32:\n";
    constexpr auto y = std::uint32_t(0xDEADBEEFu);
    dump(y);
    dump(std::byteswap(y));

    std::cout << "\nbyteswap for U64:\n";
    constexpr auto z = std::uint64_t{0x0123456789ABCDEFull};
    dump(z);
    dump(std::byteswap(z));
}
```

Possible output:

```text
byteswap for U16:
CAFE : FE CA
FECA : CA FE

byteswap for U32:
DEADBEEF : EF BE AD DE
EFBEADDE : DE AD BE EF

byteswap for U64:
0123456789ABCDEF : EF CD AB 89 67 45 23 01
EFCDAB8967452301 : 01 23 45 67 89 AB CD EF
```

### See also

- **endian (C++20)** — indicates the endianness of scalar types (enum)
- **rotl (C++20)** — computes the result of bitwise left-rotation (function
  template)
- **rotr (C++20)** — computes the result of bitwise right-rotation (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/byteswap*
