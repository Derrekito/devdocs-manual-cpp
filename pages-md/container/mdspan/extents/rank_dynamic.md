# std::extents<IndexType,Extents...>::rank_dynamic

```cpp
static constexpr rank_type rank_dynamic() const noexcept;  // (since C++23)
```

Returns the number of dynamic dimensions in `extents`.

### Parameters

(none)

### Return value

The number of dynamic dimensions.

### Example

```cpp
#include <iostream>
#include <mdspan>

int main()
{
    std::extents<int, 1, 2> e1;
    std::extents<int, 3, 4, std::dynamic_extent> e2(5);
    std::extents<int, std::dynamic_extent, 7, std::dynamic_extent> e3(6, 8);
    std::cout << e1.rank_dynamic() << ", "
              << e2.rank_dynamic() << ", "
              << e3.rank_dynamic() << '\n';
}
```

Output:

```text
0, 1, 2
```

### See also

- **rank [static]** — returns the static rank of an `extents` (public static
  member function)
- **rank (C++11)** — obtains the number of dimensions of an array type (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/extents/rank_dynamic*
