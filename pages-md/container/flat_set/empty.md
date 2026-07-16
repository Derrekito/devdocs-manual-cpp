# std::flat_set<Key,Compare,KeyContainer>::empty

```cpp
[[nodiscard]] bool empty() const noexcept;  // (since C++23)
```

Checks if the underlying container has no elements. Equivalent to `return
begin() == end();`.

### Parameters

(none)

### Return value

`true` if the underlying container is empty, `false` otherwise.

### Complexity

Constant.

### Example

The following code uses `empty` to check if a `std::flat_set<int>` contains any
elements:

```cpp
#include <iostream>
#include <flat_set>

int main()
{
    std::flat_set<int> numbers;
    std::cout << std::boolalpha;
    std::cout << "Initially, numbers.empty(): " << numbers.empty() << '\n';

    numbers.insert(42);
    numbers.insert(19937);
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
*Source: https://en.cppreference.com/w/cpp/container/flat_set/empty*
