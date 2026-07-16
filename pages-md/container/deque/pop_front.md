# std::deque<T,Allocator>::pop_front

```cpp
void pop_front();
```

Removes the first element of the container. If there are no elements in the
container, the behavior is undefined.

Iterators and references to the erased element are invalidated. If the element
is the last element in the container, the `end()` iterator is also invalidated.
Other references and iterators are not affected.

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
#include <deque>
#include <iostream>

int main()
{
    std::deque<char> chars{'A', 'B', 'C', 'D'};

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
*Source: https://en.cppreference.com/w/cpp/container/deque/pop_front*
