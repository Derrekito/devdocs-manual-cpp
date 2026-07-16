# std::vector<T,Allocator>::size

```cpp
size_type size() const;  // (until C++11)
size_type size() const noexcept;  // (since C++11) (until C++20)
constexpr size_type size() const noexcept;  // (since C++20)
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
`std::vector<int>`:

```cpp
#include <vector>
#include <iostream>

int main()
{
    std::vector<int> nums {1, 3, 5, 7};

    std::cout << "nums contains " << nums.size() << " elements.\n";
}
```

Output:

```text
nums contains 4 elements.
```

### See also

- **capacity** — returns the number of elements that can be held in currently
  allocated storage (public member function)
- **empty** — checks whether the container is empty (public member function)
- **max_size** — returns the maximum possible number of elements (public member
  function)
- **resize** — changes the number of elements stored (public member function)
- **sizessize (C++17)(C++20)** — returns the size of a container or array
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/size*
