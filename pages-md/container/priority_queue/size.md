# std::priority_queue<T,Container,Compare>::size

```cpp
size_type size() const;
```

Returns the number of elements in the underlying container, that is, `c.size()`.

### Parameters

(none)

### Return value

The number of elements in the container adaptor.

### Complexity

Constant.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <queue>

int main()
{
    std::priority_queue<int> queue;

    std::cout << "Initially, queue.size(): " << queue.size() << '\n';

    for (int i = 0; i < 8; ++i)
        queue.push(i);

    std::cout << "After adding elements, queue.size(): " << queue.size() << '\n';
}
```

Output:

```text
Initially, queue.size(): 0
After adding elements, queue.size(): 8
```

### See also

- **empty** — checks whether the container adaptor is empty (public member
  function)
- **sizessize (C++17)(C++20)** — returns the size of a container or array
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/priority_queue/size*
