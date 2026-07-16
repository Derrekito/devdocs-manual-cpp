# std::stack<T,Container>::empty

```cpp
bool empty() const;  // (until C++20)
[[nodiscard]] bool empty() const;  // (since C++20)
```

Checks if the underlying container has no elements. Equivalent to `return
c.empty();`.

### Parameters

(none)

### Return value

`true` if the underlying container is empty, `false` otherwise.

### Complexity

Constant.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <stack>

int main()
{
    std::cout << std::boolalpha;

    std::stack<int> stack;

    std::cout << "Initially, stack.empty(): " << stack.empty() << '\n';

    stack.push(42);
    std::cout << "After adding elements, stack.empty(): " << stack.empty() << '\n';
}
```

Output:

```text
Initially, stack.empty(): true
After adding elements, stack.empty(): false
```

### See also

- **size** — returns the number of elements (public member function)
- **empty (C++17)** — checks whether the container is empty (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/stack/empty*
