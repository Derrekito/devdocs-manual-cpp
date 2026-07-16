# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::end, std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::cend

```cpp
iterator end() noexcept;  // (1) (since C++11)
const_iterator end() const noexcept;  // (2) (since C++11)
const_iterator cend() const noexcept;  // (3) (since C++11)
```

Returns an iterator to the element following the last element of the
`unordered_map`.

This element acts as a placeholder; attempting to access it results in undefined
behavior.

### Parameters

(none)

### Return value

Iterator to the element following the last element.

### Complexity

Constant.

### Example

```cpp
#include <cmath>
#include <iostream>
#include <unordered_map>

struct Node { double x, y; };

int main()
{
    Node nodes[3] = {{1, 0}, {2, 0}, {3, 0}};

    // mag is a map mapping the address of a Node to its magnitude in the plane
    std::unordered_map<Node*, double> mag =
    {
        { nodes + 0, 1 },
        { nodes + 1, 2 },
        { nodes + 2, 3 }
    };

    // Change each y-coordinate from 0 to the magnitude
    for (auto iter = mag.begin(); iter != mag.end(); ++iter)
    {
        auto cur = iter->first; // pointer to Node
        cur->y = mag[cur]; // could also have used cur->y = iter->second;
    }

    // Update and print the magnitude of each node
    for (auto iter = mag.begin(); iter != mag.end(); ++iter)
    {
        auto cur = iter->first;
        mag[cur] = std::hypot(cur->x, cur->y);
        std::cout << "The magnitude of (" << cur->x << ", " << cur->y << ") is ";
        std::cout << iter->second << '\n';
    }

    // Repeat the above with the range-based for loop
    for (auto i : mag)
    {
        auto cur = i.first;
        cur->y = i.second;
        mag[cur] = std::hypot(cur->x, cur->y);
        std::cout << "The magnitude of (" << cur->x << ", " << cur->y << ") is ";
        std::cout << mag[cur] << '\n';
        // Note that in contrast to std::cout << iter->second << '\n'; above,
        // std::cout << i.second << '\n'; will NOT print the updated magnitude
    }
}
```

Possible output:

```text
The magnitude of (3, 3) is 4.24264
The magnitude of (1, 1) is 1.41421
The magnitude of (2, 2) is 2.82843
The magnitude of (3, 4.24264) is 5.19615
The magnitude of (1, 1.41421) is 1.73205
The magnitude of (2, 2.82843) is 3.4641
```

### See also

- **begincbegin** — returns an iterator to the beginning (public member
  function)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/end*
