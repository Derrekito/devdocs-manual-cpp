# operator&,|,^(std::bitset)

```cpp
template< std::size_t N >
std::bitset<N> operator&( const std::bitset<N>& lhs,
                          const std::bitset<N>& rhs );  // (until C++11)
template< std::size_t N >
std::bitset<N> operator&( const std::bitset<N>& lhs,
                          const std::bitset<N>& rhs ) noexcept;  // (since C++11) (constexpr since C++23)
template< std::size_t N >
std::bitset<N> operator|( const std::bitset<N>& lhs,
                          const std::bitset<N>& rhs );  // (until C++11)
template< std::size_t N >
std::bitset<N> operator|( const std::bitset<N>& lhs,
                          const std::bitset<N>& rhs ) noexcept;  // (since C++11) (constexpr since C++23)
template< std::size_t N >
std::bitset<N> operator^( const std::bitset<N>& lhs,
                          const std::bitset<N>& rhs );  // (until C++11)
template< std::size_t N >
std::bitset<N> operator^( const std::bitset<N>& lhs,
                          const std::bitset<N>& rhs ) noexcept;  // (since C++11) (constexpr since C++23)
```

Performs binary AND, OR, and XOR between two bitsets, `lhs` and `rhs`.

1) Returns a `std::bitset<N>` containing the result of binary AND on
   corresponding pairs of bits of `lhs` and `rhs`.

2) Returns a `std::bitset<N>` containing the result of binary OR on
   corresponding pairs of bits of `lhs` and `rhs`.

3) Returns a `std::bitset<N>` containing the result of binary XOR on
   corresponding pairs of bits of `lhs` and `rhs`.

### Parameters

- **lhs** — the bitset on the left-hand side of the operator
- **rhs** — the bitset on the right-hand side of the operator

### Return value

1) `std::bitset<N>(lhs) &= rhs`

2) `std::bitset<N>(lhs) |= rhs`

3) `std::bitset<N>(lhs) ^= rhs`

### Example

```cpp
#include <bitset>
#include <iostream>

int main()
{
    std::bitset<4> b1("0110");
    std::bitset<4> b2("0011");

    std::cout << "b1 & b2: " << (b1 & b2) << '\n';
    std::cout << "b1 | b2: " << (b1 | b2) << '\n';
    std::cout << "b1 ^ b2: " << (b1 ^ b2) << '\n';
}
```

Output:

```text
b1 & b2: 0010
b1 | b2: 0111
b1 ^ b2: 0101
```

### See also

- **operator&=operator|=operator^=operator~** — performs binary AND, OR, XOR and
  NOT (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/bitset/operator_logic2*
