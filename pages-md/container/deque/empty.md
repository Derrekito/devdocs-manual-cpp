# std::deque<T,Allocator>::empty

```cpp
bool empty() const;  // (until C++11)
bool empty() const noexcept;  // (since C++11) (until C++20)
[[nodiscard]] bool empty() const noexcept;  // (since C++20)
```

Checks if the container has no elements, i.e. whether `begin() == end()`.

### Parameters

(none)

### Return value

`true` if the container is empty, `false` otherwise.

### Complexity

Constant.

### Example

The following code uses `empty` to check if a `std::deque<int>` contains any
elements:

```cpp
#include <deque>
#include <iostream>

int main()
{
    std::deque<int> numbers;
    std::cout << std::boolalpha;
    std::cout << "Initially, numbers.empty(): " << numbers.empty() << '\n';

    numbers.push_back(42);
    numbers.push_back(13317);
    std::cout << "After adding elements, numbers.empty(): " << numbers.empty() << '\n';
}
```

Output:

```text
Initially, numbers.empty(): true
After adding elements, numbers.empty(): false
```

### See also

- **size** — returns the number of elements (public member function)
- **empty (C++17)** — checks whether the container is empty (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/deque/empty*
