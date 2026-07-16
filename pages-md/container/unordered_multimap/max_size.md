# std::unordered_multimap<Key,T,Hash,KeyEqual,Allocator>::max_size

```cpp
size_type max_size() const noexcept;  // (since C++11)
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
#include <unordered_map>

int main()
{
    std::unordered_multimap<char, char> q;
    std::cout.imbue(std::locale("en_US.UTF-8"));
    std::cout << "Maximum size of a std::unordered_multimap is " << q.max_size() << '\n';
}
```

Possible output:

```text
Maximum size of a std::unordered_multimap is 768,614,336,404,564,650
```

### See also

- **size** — returns the number of elements (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multimap/max_size*
