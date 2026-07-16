# std::array<T,N>::fill

```cpp
void fill( const T& value );  // (since C++11) (constexpr since C++20)
```

Assigns the `value` to all elements in the container.

### Parameters

- **value** — the value to assign to the elements

### Return value

(none)

### Complexity

Linear in the size of the container.

### Example

```cpp
#include <array>
#include <cstddef>
#include <iostream>

int main()
{
    constexpr std::size_t xy = 4;

    using Cell = std::array<unsigned char, 8>;

    std::array<Cell, xy * xy> board;

    board.fill({0xE2, 0x96, 0x84, 0xE2, 0x96, 0x80, 0, 0}); // "▄▀";

    for (std::size_t count{}; Cell c : board)
        std::cout << c.data() << ((++count % xy) ? "" : "\n");
}
```

Possible output:

```text
▄▀▄▀▄▀▄▀
▄▀▄▀▄▀▄▀
▄▀▄▀▄▀▄▀
▄▀▄▀▄▀▄▀
```

### See also

- **fill** — copy-assigns the given value to every element in a range (function
  template)
- **fill_n** — copy-assigns the given value to N elements in a range (function
  template)
- **ranges::fill (C++20)** — assigns a range of elements a certain value
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/container/array/fill*
