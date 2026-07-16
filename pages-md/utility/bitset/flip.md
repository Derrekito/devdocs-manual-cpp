# std::bitset<N>::flip

```cpp
bitset& flip();  // (until C++11)
bitset& flip() noexcept;  // (since C++11) (constexpr since C++23)
bitset& flip( std::size_t pos );  // (2) (constexpr since C++23)
```

Flips bits, i.e. changes `true` values to `false` and `false` values to `true`.
Equivalent to a logical NOT operation on part or all of the bitset.

1) Flips all bits (like `operator~`, but in-place).

2) Flips the bit at the position `pos`.

### Parameters

- **pos** — the position of the bit to flip

### Return value

`*this`

### Exceptions

2) throws `std::out_of_range` if `pos` does not correspond to a valid position
   within the bitset.

### Example

```cpp
#include <bitset>
#include <iostream>

int main()
{
    std::bitset<4> flops;

    std::cout << flops << '\n'
              << flops.flip(0) << '\n'
              << flops.flip(2) << '\n'
              << flops.flip() << '\n';
}
```

Output:

```text
0000
0001
0101
1010
```

### See also

- **set** — sets bits to `true` or given value (public member function)
- **reset** — sets bits to `false` (public member function)
- **operator&=operator|=operator^=operator~** — performs binary AND, OR, XOR and
  NOT (public member function)
- **flip** — flips all the bits (public member function of
  `std::vector<bool,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/utility/bitset/flip*
