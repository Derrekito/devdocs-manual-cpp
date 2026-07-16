# std::bit_ceil

```cpp
template< class T >
constexpr T bit_ceil( T x );  // (since C++20)
```

Calculates the smallest integral power of two that is not smaller than `x`.

If that value is not representable in `T`, the behavior is undefined. Call to
this function is permitted in constant evaluation only if the undefined behavior
does not occur.

This overload participates in overload resolution only if `T` is an unsigned
integer type (that is, `unsigned char`, `unsigned short`, `unsigned int`,
`unsigned long`, `unsigned long long`, or an extended unsigned integer type).

### Parameters

- **x** — value of unsigned integer type

### Return value

The smallest integral power of two that is not smaller than `x`.

### Exceptions

Throws nothing.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_int_pow2` | 202002L | (C++20) | Integral power-of-2 operations

### Possible implementation

See possible implementations in libstdc++ (gcc) and libc++ (clang).

```cpp
template<std::unsigned_integral T>
    requires !std::same_as<T, bool> && !std::same_as<T, char> &&
             !std::same_as<T, char8_t> && !std::same_as<T, char16_t> &&
             !std::same_as<T, char32_t> && !std::same_as<T, wchar_t>
constexpr T bit_ceil(T x) noexcept
{
    if (x <= 1u)
        return T(1);
    if constexpr (std::same_as<T, decltype(+x)>)
        return T(1) << std::bit_width(T(x - 1));
    else
    {   // for types subject to integral promotion
        constexpr int offset_for_ub =
            std::numeric_limits<unsigned>::digits - std::numeric_limits<T>::digits;
        return T(1u << (std::bit_width(T(x - 1)) + offset_for_ub) >> offset_for_ub);
    }
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

    for (unsigned x{0}; x != 10; ++x)
    {
        unsigned const z = std::bit_ceil(x); // `ceil2` before P1956R1
        std::cout << "bit_ceil( " << bin(x) << " ) = " << bin(z) << '\n';
    }
}
```

Output:

```text
bit_ceil( 00000000 ) = 00000001
bit_ceil( 00000001 ) = 00000001
bit_ceil( 00000010 ) = 00000010
bit_ceil( 00000011 ) = 00000100
bit_ceil( 00000100 ) = 00000100
bit_ceil( 00000101 ) = 00001000
bit_ceil( 00000110 ) = 00001000
bit_ceil( 00000111 ) = 00001000
bit_ceil( 00001000 ) = 00001000
bit_ceil( 00001001 ) = 00010000
```

### See also

- **bit_floor (C++20)** — finds the largest integral power of two not greater
  than the given value (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/bit_ceil*
