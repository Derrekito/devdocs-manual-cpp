# std::bitset<N>::operator[]

```cpp
bool operator[]( std::size_t pos ) const;  // (1) (constexpr since C++11)
reference operator[]( std::size_t pos );  // (2) (constexpr since C++23)
```

Accesses the bit at position `pos`. The first version returns the value of the
bit, the second version returns an object of type `std::bitset::reference` that
allows modification of the value.

Unlike `test()`, does not throw exceptions: the behavior is undefined if `pos`
is out of bounds.

### Parameters

- **pos** — position of the bit to return

### Return value

1) The value of the requested bit.

2) An object of type `std::bitset::reference`, which allows writing to the
   requested bit.

### Exceptions

(none)

### Example

```cpp
#include <bitset>
#include <cstddef>
#include <iostream>

int main()
{
    std::bitset<8> b1{0b00101010}; // binary literal for 42

    for (std::size_t i = 0; i < b1.size(); ++i)
        std::cout << "b1[" << i << "]: " << b1[i] << '\n';
    b1[0] = true; // modifies the first bit through bitset::reference

    std::cout << "After setting bit 0, b1 holds " << b1 << '\n';
}
```

Output:

```text
b1[0]: 0
b1[1]: 1
b1[2]: 0
b1[3]: 1
b1[4]: 0
b1[5]: 1
b1[6]: 0
b1[7]: 0
After setting bit 0, b1 holds 00101011
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 11 | C++98 | 1. the description was missing in the C++ standard 2. there
      was only the non-const overload | 1. description added 2. added the const
      overload
  LWG 907 | C++98 | the behavior of reading the bit at `pos` was equivalent to
      that of `test(pos)`, but `test()` may throw exceptions | avoids mentioning
      `test()`

### See also

- **test** — accesses specific bit (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/bitset/operator_at*
