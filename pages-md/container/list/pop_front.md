# std::list<T,Allocator>::pop_front

```cpp
void pop_front();
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
#include <list>
#include <iostream>

int main()
{
    std::list<char> chars{'A', 'B', 'C', 'D'};

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

- **pop_back** — removes the last element (public member function)
- **push_front** — inserts an element to the beginning (public member function)
- **front** — access the first element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/list/pop_front*
