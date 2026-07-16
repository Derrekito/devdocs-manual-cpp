# std::flat_map<Key,T,Compare,KeyContainer,MappedContainer>::max_size

```cpp
size_type max_size() const noexcept;  // (since C++23)
```

Returns the maximum number of elements the container is able to hold due to
system or library implementation limitations. Equivalent to
`std::min<size_type>(c.keys.max_size(), c.values.max_size())`.

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
#include <flat_map>
#include <locale>

int main()
{
    std::flat_map<char, char> q;
    std::cout.imbue(std::locale("en_US.UTF-8"));
    std::cout << "Maximum size of a std::flat_map is " << q.max_size() << '\n';
}
```

Possible output:

```text
Maximum size of a std::flat_map is 768,614,336,404,564,650
```

### See also

- **size** — returns the number of elements (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/flat_map/max_size*
