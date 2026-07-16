# std::flat_map<Key,T,Compare,KeyContainer,MappedContainer>::size

```cpp
size_type size() const noexcept;  // (since C++23)
```

Returns the number of elements in the container adaptor, that is,
`std::distance(begin(), end())`.

### Parameters

(none)

### Return value

The number of elements in the container adaptor.

### Complexity

Constant.

### Example

The following code uses `size` to display the number of elements in a
`std::flat_map`:

```cpp
#include <iostream>
#include <flat_map>

int main()
{
    std::flat_map<int,char> nums{{1, 'a'}, {3, 'b'}, {5, 'c'}, {7, 'd'}};

    std::cout << "nums contains " << nums.size() << " elements.\n";
}
```

Output:

```text
nums contains 4 elements.
```

### See also

- **empty** — checks whether the container adaptor is empty (public member
  function)
- **sizessize (C++17)(C++20)** — returns the size of a container or array
  (function template)
- **max_size** — returns the maximum possible number of elements (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/container/flat_map/size*
