# std::array<T,N>::front

```cpp
reference front();  // (1) (since C++11) (constexpr since C++17)
const_reference front() const;  // (2) (since C++11) (constexpr since C++14)
```

Returns a reference to the first element in the container.

Calling `front` on an empty container causes undefined behavior.

### Parameters

(none)

### Return value

Reference to the first element.

### Complexity

Constant.

### Notes

For a container `c`, the expression `c.front()` is equivalent to `*c.begin()`.

### Example

The following code uses `front` to display the first element of a
`std::array<char, 6>`:

```cpp
#include <array>
#include <iostream>

int main()
{
    std::array<char, 6> letters{'a', 'b', 'c', 'd', 'e', 'f'};

    if (!letters.empty())
        std::cout << "The first character is '" << letters.front() << "'.\n";
}
```

Output:

```text
The first character is 'a'.
```

### See also

- **back** — access the last element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/array/front*
