# std::vector<T,Allocator>::max_size

```cpp
size_type max_size() const;  // (until C++11)
size_type max_size() const noexcept;  // (since C++11) (until C++20)
constexpr size_type max_size() const noexcept;  // (since C++20)
```

Returns the maximum number of elements the container is able to hold due to
system or library implementation limitations, i.e. `std::distance(begin(),
end())` for the largest container.

### Parameters

(none)

### Return value

Maximum number of elements.

### Complexity

Constant.

### Notes

This value typically reflects the theoretical limit on the size of the
container, at most `std::numeric_limits<difference_type>::max()`. At runtime,
the size of the container may be limited to a value smaller than `max_size()` by
the amount of RAM available.

### Example

```cpp
#include <iostream>
#include <locale>
#include <vector>

int main()
{
    std::vector<char> q;
    std::cout.imbue(std::locale("en_US.UTF-8"));
    std::cout << "Maximum size of a std::vector is " << q.max_size() << '\n';
}
```

Possible output:

```text
Maximum size of a std::vector is 9,223,372,036,854,775,807
```

### See also

- **size** — returns the number of elements (public member function)
- **capacity** — returns the number of elements that can be held in currently
  allocated storage (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/max_size*
