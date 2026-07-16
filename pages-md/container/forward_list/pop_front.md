# std::forward_list<T,Allocator>::pop_front

```cpp
void pop_front();  // (since C++11)
```

Removes the first element of the container. If there are no elements in the
container, the behavior is undefined.

References and iterators to the erased element are invalidated.

### Parameters

(none)

### Return value

(none)

### Complexity

Constant.

### Exceptions

Does not throw.

### Example

```cpp
#include <forward_list>
#include <iostream>

int main()
{
    std::forward_list<char> chars{'A', 'B', 'C', 'D'};

    for (; !chars.empty(); chars.pop_front())
        std::cout << "chars.front(): '" << chars.front() << "'\n";
}
```

Output:

```text
chars.front(): 'A'
chars.front(): 'B'
chars.front(): 'C'
chars.front(): 'D'
```

### See also

- **push_front** — inserts an element to the beginning (public member function)
- **front** — access the first element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/forward_list/pop_front*
