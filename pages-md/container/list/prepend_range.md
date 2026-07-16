# std::list<T,Allocator>::prepend_range

```cpp
template< container-compatible-range<T> R >
void prepend_range( R&& rg );  // (since C++23)
```

Inserts, in non-reversing order, copies of elements in `rg` before `begin()`.
Each iterator in the range `rg` is dereferenced exactly once.

No iterators or references are invalidated.

### Parameters

- **rg** — a container compatible range, that is, an `input_range` whose
  elements are convertible to `T`

**Type requirements**

**-`T` must be EmplaceConstructible into `list` from `*ranges::begin(rg)`. Otherwise, the behavior is undefined.**

### Return value

(none)

### Complexity

Linear in size of `rg`.

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
    auto container = std::list{0, 1, 2, 3};
    const auto rg = std::vector{-3, -2, -1};

#if __cpp_lib_containers_ranges
    container.prepend_range(rg);
#else
    container.insert(container.begin(), rg.cbegin(), rg.cend());
#endif
    assert(std::ranges::equal(container, std::list{-3, -2, -1, 0, 1, 2, 3}));
}
```

### See also

- **append_range (C++23)** — adds a range of elements to the end (public member
  function)
- **insert_range (C++23)** — inserts a range of elements (public member
  function)
- **push_front** — inserts an element to the beginning (public member function)
- **emplace_front (C++11)** — constructs an element in-place at the beginning
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/list/prepend_range*
