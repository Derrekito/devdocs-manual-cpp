# std::extents<IndexType,Extents...>::static_extent

```cpp
static constexpr std::size_t static_extent( rank_type i ) noexcept;  // (since C++23)
```

Returns static extent size of an `extents` at the rank index `i`. If the rank
index `i` is a dynamic extent, returns `std::dynamic_extent`.

### Parameters

- **i** — The rank index to get the static extent size of

### Return value

The static extent size or `std::dynamic_extent` value.

### Example

```cpp
#include <iostream>
#include <mdspan>

int main()
{
    std::extents<int, 1, 2> e1;
    std::extents<int, 3, std::dynamic_extent, std::dynamic_extent> e2(4, 5);
    std::cout << e1.static_extent(0) << ", " << e1.static_extent(1) << '\n';
    std::cout << (e2.static_extent(0) == std::dynamic_extent) << ", "
              << (e2.static_extent(1) == std::dynamic_extent) << ", "
              << (e2.static_extent(2) == std::dynamic_extent) << '\n';
}
```

Output:

```text
1, 2
0, 1, 1
```

### See also

- **extent** — returns dynamic extent size of an `extents` at a certain rank
  index (public member function)
- **extent (C++11)** — obtains the size of an array type along a specified
  dimension (class template)

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/extents/static_extent*
