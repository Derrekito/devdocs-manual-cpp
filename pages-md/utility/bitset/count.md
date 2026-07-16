# std::bitset<N>::count

```cpp
std::size_t count() const;  // (until C++11)
std::size_t count() const noexcept;  // (since C++11) (constexpr since C++23)
```

Returns the number of bits that are set to `true`.

### Parameters

(none)

### Return value

Number of bits that are set to `true`.

### Example

```cpp
#include <bitset>
#include <iostream>

int main()
{
    std::bitset<8> b("00010010");
    std::cout << "Initial value: " << b << '\n';

    // Find the first unset bit
    std::size_t idx = 0;
    while (idx < b.size() && b.test(idx))
        ++idx;

    // Continue setting bits until half the bitset is filled
    while (idx < b.size() && b.count() < b.size() / 2)
    {
        b.set(idx);
        std::cout << "Setting bit " << idx << ": " << b << '\n';
        while (idx < b.size() && b.test(idx))
            ++idx;
    }

}
```

Output:

```text
Initial value: 00010010
Setting bit 0: 00010011
Setting bit 2: 00010111
```

### See also

- **size** — returns the number of bits that the bitset holds (public member
  function)
- **popcount (C++20)** — counts the number of `1` bits in an unsigned integer
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/bitset/count*
