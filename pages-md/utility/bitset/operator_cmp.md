# std::bitset<N>::operator==, std::bitset<N>::operator!=

```cpp
bool operator==( const bitset& rhs ) const;  // (until C++11)
bool operator==( const bitset& rhs ) const noexcept;  // (since C++11) (constexpr since C++23)
bool operator!=( const bitset& rhs ) const;  // (until C++11)
bool operator!=( const bitset& rhs ) const noexcept;  // (since C++11) (until C++20)
```

1) Returns true if all of the bits in `*this` and `rhs` are equal.

2) Returns true if any of the bits in `*this` and `rhs` are not equal.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Parameters

- **rhs** — bitset to compare

### Return value

1) `true` if the value of each bit in `*this` equals the value of the
   corresponding bit in `rhs`, otherwise `false`.

2) `true` if `!(*this == rhs)`, otherwise `false`.

### Example

Compare given bitsets to determine if they are identical:

```cpp
#include <bitset>
#include <iostream>

int main()
{
    std::bitset<4> b1(0b0011);
    std::bitset<4> b2(b1);
    std::bitset<4> b3(0b0100);

    std::cout << std::boolalpha;
    std::cout << "b1 == b2: " << (b1 == b2) << '\n';
    std::cout << "b1 == b3: " << (b1 == b3) << '\n';
    std::cout << "b1 != b3: " << (b1 != b3) << '\n';

//  b1 == std::bitset<3>{}; // compile-time error: incompatible types
}
```

Output:

```text
b1 == b2: true
b1 == b3: false
b1 != b3: true
```

---
*Source: https://en.cppreference.com/w/cpp/utility/bitset/operator_cmp*
