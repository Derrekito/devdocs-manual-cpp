# std::map<Key,T,Compare,Allocator>::size

```cpp
size_type size() const;  // (until C++11)
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
`std::map`:

```cpp
#include <iostream>
#include <map>

int main()
{
    std::map<int,char> nums{{1, 'a'}, {3, 'b'}, {5, 'c'}, {7, 'd'}};

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
*Source: https://en.cppreference.com/w/cpp/container/map/size*
