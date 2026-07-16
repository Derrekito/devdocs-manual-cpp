# std::priority_queue<T,Container,Compare>::empty

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
#include <queue>

int main()
{
    std::cout << std::boolalpha;

    std::priority_queue<int> queue;

    std::cout << "Initially, queue.empty(): " << queue.empty() << '\n';

    queue.push(42);
    std::cout << "After adding elements, queue.empty(): " << queue.empty() << '\n';
}
```

Output:

```text
Initially, queue.empty(): true
After adding elements, queue.empty(): false
```

### See also

- **size** — returns the number of elements (public member function)
- **empty (C++17)** — checks whether the container is empty (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/priority_queue/empty*
