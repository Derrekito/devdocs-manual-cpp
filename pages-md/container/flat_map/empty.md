# std::flat_map<Key,T,Compare,KeyContainer,MappedContainer>::empty

```cpp
[[nodiscard]] bool empty() const noexcept;  // (since C++23)
```

Checks if the underlying containers have no elements. Equivalent to `return
begin() == end();`.

### Parameters

(none)

### Return value

`true` if the underlying containers are empty, `false` otherwise.

### Complexity

Constant.

### Example

The following code uses `empty` to check if a `std::flat_map<int, int>` contains
any elements:

```cpp
#include <iostream>
#include <flat_map>
#include <utility>

int main()
{
    std::flat_map<int,int> numbers;
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
*Source: https://en.cppreference.com/w/cpp/container/flat_map/empty*
