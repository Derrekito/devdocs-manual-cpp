# std::forward_list<T,Allocator>::front

```cpp
reference front();  // (1) (since C++11)
const_reference front() const;  // (2) (since C++11)
```

Returns a reference to the first element in the container.

Calling `front` on an empty container causes undefined behavior.

### Parameters

(none)

### Return value

Reference to the first element.

### Complexity

Constant.

### Notes

For a container `c`, the expression `c.front()` is equivalent to `*c.begin()`.

### Example

The following code uses `front` to display the first element of a
`std::forward_list<char>`:

```cpp
#include <forward_list>
#include <iostream>

int main()
{
    std::forward_list<char> letters{'a', 'b', 'c', 'd', 'e', 'f'};

    if (!letters.empty())
        std::cout << "The first character is '" << letters.front() << "'.\n";
}
```

Output:

```text
The first character is 'a'.
```

### See also

- **push_front** — inserts an element to the beginning (public member function)
- **pop_front** — removes the first element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/forward_list/front*
