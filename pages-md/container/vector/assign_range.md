# std::vector<T,Allocator>::assign_range

```cpp
template< container-compatible-range<T> R >
constexpr void assign_range( R&& rg );  // (since C++23)
```

Replaces elements in the container with a copy of each element in `rg`.

All iterators (including the `end()` iterator) and references to the elements
are invalidated.

Each iterator in the range `rg` is dereferenced exactly once.

The behavior is undefined if `rg` overlaps with the container.

### Parameters

- **rg** — an `input_range` with reference type convertible to the element type
  of the container

**Type requirements**

**-`std::assignable_from<T&, ranges::range_reference_t<R>>` must be modeled. Otherwise, the program is ill-formed.**

**-`T` must be EmplaceConstructible into the container from `*ranges::begin(rg)`. If `R` models neither `sized_range` nor `forward_range`, `T` must be also MoveInsertable into the container. Otherwise, the behavior is undefined.**

### Return value

(none)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges-aware construction
      and insertion

### Example

```cpp
#include <algorithm>
#include <cassert>
#include <vector>
#include <list>

int main()
{
    const auto source = std::list{2, 7, 1};
    auto destination = std::vector{3, 1, 4};
#ifdef __cpp_lib_containers_ranges
    destination.assign_range(source);
#else
    destination.assign(source.cbegin(), source.cend());
#endif
    assert(std::ranges::equal(source, destination));
}
```

### See also

- **insert_range (C++23)** — inserts a range of elements (public member
  function)
- **append_range (C++23)** — adds a range of elements to the end (public member
  function)
- **assign** — assigns values to the container (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/assign_range*
