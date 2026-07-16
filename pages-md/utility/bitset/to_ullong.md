# std::bitset<N>::to_ullong

```cpp
unsigned long long to_ullong() const  // (since C++11) (constexpr since C++23)
```

Converts the contents of the bitset to an `unsigned long long` integer.

The first bit of the bitset corresponds to the least significant digit of the
number and the last bit corresponds to the most significant digit.

### Parameters

(none)

### Return value

The converted integer

### Exceptions

`std::overflow_error` if the value can not be represented in `unsigned long
long`.

### Example

```cpp
#include <bitset>
#include <iostream>
#include <limits>

int main()
{
    std::bitset<std::numeric_limits<unsigned long long>::digits> b
    (
        0x123456789abcdef0LL
    );

    std::cout << b << "  " << std::hex << b.to_ullong() << '\n';
    b.flip();
    std::cout << b << "  " << b.to_ullong() << '\n';

    std::bitset<std::numeric_limits<unsigned long long>::digits + 1> q{0};
    try
    {
        (~q).to_ullong(); // throws
    }
    catch (const std::overflow_error& ex)
    {
        std::cout << "ex: " << ex.what() << '\n';
    }
}
```

Output:

```text
0001001000110100010101100111100010011010101111001101111011110000  123456789abcdef0
1110110111001011101010011000011101100101010000110010000100001111  edcba9876543210f
ex: _Base_bitset::_M_do_to_ullong
```

### See also

- **to_string** — returns a string representation of the data (public member
  function)
- **to_ulong** — returns an unsigned long integer representation of the data
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/bitset/to_ullong*
