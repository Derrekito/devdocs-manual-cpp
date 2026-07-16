# std::bitset<N>::size

```cpp
std::size_t size() const;  // (until C++11)
constexpr std::size_t size() const noexcept;  // (since C++11)
```

Returns the number of bits that the bitset holds.

### Parameters

(none)

### Return value

number of bits that the bitset holds, i.e. the template parameter `N`.

### Example

```cpp
#include <bitset>
#include <iostream>

int main()
{
    std::cout << std::bitset<0x400>().size() << '\n';
}
```

Output:

```text
1024
```

### See also

- **count** — returns the number of bits set to `true` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/bitset/size*
