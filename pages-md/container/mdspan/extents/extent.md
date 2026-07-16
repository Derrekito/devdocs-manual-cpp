# std::extents<IndexType,Extents...>::extent

```cpp
constexpr index_type extent( rank_type i ) const noexcept;  // (since C++23)
```

Returns dynamic extent size of an `extents` at a certain rank index.

### Parameters

- **i** — The rank index to get the extent size of

### Return value

The dynamic extent size of an `extents` at a certain rank index.

### Example

```cpp
#include <iostream>
#include <mdspan>

int main()
{
    std::extents<int, 1, 2> e1;
    std::extents<int, 3, std::dynamic_extent, std::dynamic_extent> e2(4, 5);
    std::cout << e1.extent(0) << ", " << e1.extent(1) << '\n';
    std::cout << e2.extent(0) << ", " << e2.extent(1) << ", " << e2.extent(2) << '\n';
}
```

Output:

```text
1, 2
3, 4, 5
```

### See also

- **static_extent [static]** — returns static extent size of an `extents` at a
  certain rank index (public static member function)
- **extent (C++11)** — obtains the size of an array type along a specified
  dimension (class template)

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/extents/extent*
