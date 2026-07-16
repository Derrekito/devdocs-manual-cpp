# std::vector<bool,Allocator>::flip

```cpp
void flip();  // (until C++20)
constexpr void flip();  // (since C++20)
```

Toggles each `bool` in the vector (replaces with its opposite value).

### Parameters

(none)

### Return value

(none)

### Example

```cpp
#include <iostream>
#include <vector>

void print(const std::vector<bool>& vb)
{
    for (const bool b : vb)
        std::cout << b;
    std::cout << '\n';
}

int main()
{
    std::vector<bool> v{0, 1, 0, 1};
    print(v);
    v.flip();
    print(v);
}
```

Output:

```text
0101
1010
```

### See also

- **operator[]** — access specified element (public member function of
  `std::vector<T,Allocator>`)
- **flip** — toggles the values of bits (public member function of
  `std::bitset<N>`)

---
*Source: https://en.cppreference.com/w/cpp/container/vector_bool/flip*
