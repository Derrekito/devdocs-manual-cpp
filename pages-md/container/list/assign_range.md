# std::list<T,Allocator>::assign_range

```cpp
template< container-compatible-range<T> R >
void assign_range( R&& rg );  // (since C++23)
```

Replaces elements in the container with a copy of each element in `rg`.

Invalidates all iterators, pointers and references to the elements.

Each iterator in the range `rg` is dereferenced exactly once.

The behavior is undefined if `rg` overlaps with the container.

### Parameters

- **rg** — an `input_range` with reference type convertible to the element type
  of the container

**Type requirements**

**-`std::assignable_from<T&, ranges::range_reference_t<R>>` must be modeled. Otherwise, the program is ill-formed.**

**-`T` must be EmplaceConstructible into the container from `*ranges::begin(rg)`. Otherwise, the behavior is undefined.**

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
#include <list>
#include <vector>

int main()
{
    const auto source = std::vector{2, 7, 1};
    auto destination = std::list{3, 1, 4};
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
- **prepend_range (C++23)** — adds a range of elements to the beginning (public
  member function)
- **append_range (C++23)** — adds a range of elements to the end (public member
  function)
- **assign** — assigns values to the container (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/list/assign_range*
