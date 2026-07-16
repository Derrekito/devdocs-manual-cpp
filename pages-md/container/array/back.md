# std::array<T,N>::back

```cpp
reference back();  // (1) (since C++11) (constexpr since C++17)
const_reference back() const;  // (2) (since C++11) (constexpr since C++14)
```

Returns a reference to the last element in the container.

Calling `back` on an empty container causes undefined behavior.

### Parameters

(none)

### Return value

Reference to the last element.

### Complexity

Constant.

### Notes

For a non-empty container `c`, the expression `c.back()` is equivalent to
`*std::prev(c.end())`.

### Example

The following code uses `back` to display the last element of a
`std::array<char>`:

```cpp
#include <array>
#include <iostream>

int main()
{
    std::array<char, 6> letters{'a', 'b', 'c', 'd', 'e', 'f'};

    if (!letters.empty())
        std::cout << "The last character is '" << letters.back() << "'.\n";
}
```

Output:

```text
The last character is 'f'.
```

### See also

- **front** — access the first element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/array/back*
