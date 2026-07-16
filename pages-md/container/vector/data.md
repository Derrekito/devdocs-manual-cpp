# std::vector<T,Allocator>::data

```cpp
T* data();  // (until C++11)
T* data() noexcept;  // (since C++11) (constexpr since C++20)
T* data();  // (until C++11)
const T* data() const;  // (since C++11) (constexpr since C++20)
```

Returns pointer to the underlying array serving as element storage. The pointer
is such that range `[``data()``,``data() + size()``)` is always a valid range,
even if the container is empty (`data()` is not dereferenceable in that case).

### Parameters

(none)

### Return value

Pointer to the underlying element storage. For non-empty containers, the
returned pointer compares equal to the address of the first element.

### Complexity

Constant.

### Notes

If `size()` is `​0​`, `data()` may or may not return a null pointer.

### Example

```cpp
#include <cstddef>
#include <iostream>
#include <span>
#include <vector>

void pointer_func(const int* p, std::size_t size)
{
    std::cout << "data = ";
    for (std::size_t i = 0; i < size; ++i)
        std::cout << p[i] << ' ';
    std::cout << '\n';
}

void span_func(std::span<const int> data) // since C++20
{
    std::cout << "data = ";
    for (const int e : data)
        std::cout << e << ' ';
    std::cout << '\n';
}

int main()
{
    std::vector<int> container{1, 2, 3, 4};

    // Prefer container.data() over &container[0]
    pointer_func(container.data(), container.size());

    // std::span (C++20) is a safer alternative to separated pointer/size.
    span_func({container.data(), container.size()});
}
```

Output:

```text
data = 1 2 3 4
data = 1 2 3 4
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 464 | C++98 | `vector` did not have this member function | added
  LWG 1312 | C++98 | the return type were `pointer` and `const_pointer` |
      changed to T* and const T* respectively

### See also

- **front** — access the first element (public member function)
- **back** — access the last element (public member function)
- **size** — returns the number of elements (public member function)
- **span (C++20)** — a non-owning view over a contiguous sequence of objects
  (class template)
- **data (C++17)** — obtains the pointer to the underlying array (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/data*
