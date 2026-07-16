# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::size

```cpp
size_type size() const noexcept;  // (since C++11)
```

Returns the number of elements in the container, i.e. `std::distance(begin(),
end())`.

### Parameters

(none)

### Return value

The number of elements in the container.

### Complexity

Constant.

### Example

The following code uses `size` to display the number of elements in a
`std::unordered_map`:

```cpp
#include <iostream>
#include <unordered_map>

int main()
{
    std::unordered_map<int,char> nums{{1, 'a'}, {3, 'b'}, {5, 'c'}, {7, 'd'}};

    std::cout << "nums contains " << nums.size() << " elements.\n";
}
```

Output:

```text
nums contains 4 elements.
```

### See also

- **empty** — checks whether the container is empty (public member function)
- **max_size** — returns the maximum possible number of elements (public member
  function)
- **sizessize (C++17)(C++20)** — returns the size of a container or array
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/size*
