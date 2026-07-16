# std::array<T,N>::empty

```cpp
constexpr bool empty() const noexcept;  // (since C++11) (until C++20)
[[nodiscard]] constexpr bool empty() const noexcept;  // (since C++20)
```

Checks if the container has no elements, i.e. whether `begin() == end()`.

### Parameters

(none)

### Return value

`true` if the container is empty, `false` otherwise.

### Complexity

Constant.

### Example

The following code uses `empty` to check if a `std::array` contains any
elements:

```cpp
#include <array>
#include <iostream>

int main()
{
    std::array<int, 4> numbers{3, 1, 4, 1};
    std::array<int, 0> no_numbers;

    std::cout << std::boolalpha;
    std::cout << "numbers.empty(): " << numbers.empty() << '\n';
    std::cout << "no_numbers.empty(): " << no_numbers.empty() << '\n';
}
```

Output:

```text
numbers.empty(): false
no_numbers.empty(): true
```

### See also

- **size** — returns the number of elements (public member function)
- **empty (C++17)** — checks whether the container is empty (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/array/empty*
