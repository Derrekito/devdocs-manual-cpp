# std::stack<T,Container>::size

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
#include <stack>

int main()
{
    std::stack<int> stack;

    std::cout << "Initially, stack.size(): " << stack.size() << '\n';

    for (int i = 0; i < 8; ++i)
        stack.push(i);

    std::cout << "After adding elements, stack.size(): " << stack.size() << '\n';
}
```

Output:

```text
Initially, stack.size(): 0
After adding elements, stack.size(): 8
```

### See also

- **empty** — checks whether the container adaptor is empty (public member
  function)
- **sizessize (C++17)(C++20)** — returns the size of a container or array
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/stack/size*
