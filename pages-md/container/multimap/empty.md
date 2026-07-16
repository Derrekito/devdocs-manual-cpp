# std::multimap<Key,T,Compare,Allocator>::empty

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

The following code uses `empty` to check if a `std::multimap<int, int>` contains
any elements:

```cpp
#include <iostream>
#include <map>
#include <utility>

int main()
{
    std::multimap<int,int> numbers;
    std::cout << std::boolalpha;
    std::cout << "Initially, numbers.empty(): " << numbers.empty() << '\n';

    numbers.emplace(42, 13);
    numbers.insert(std::make_pair(13317, 123));
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
*Source: https://en.cppreference.com/w/cpp/container/multimap/empty*
