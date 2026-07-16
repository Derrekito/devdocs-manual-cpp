# std::list<T,Allocator>::append_range

```cpp
template< container-compatible-range<T> R >
void append_range( R&& rg );  // (since C++23)
```

Inserts copies of elements from the range `rg` before `end()`, in non-reversing
order.

No iterators or references are invalidated.

Each iterator in `rg` is dereferenced exactly once.

### Parameters

- **rg** — a container compatible range, that is, an `input_range` whose
  elements are convertible to `T`

**Type requirements**

**-`T` must be EmplaceConstructible into `list` from `*ranges::begin(rg)`. Otherwise, the behavior is undefined.**

### Return value

(none)

### Complexity

Linear in size of `rg`. The number of calls to the constructor of `T` is exactly
equal to the `std::ranges::size(rg))`.

### Exceptions

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

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
    auto head = std::list{1, 2, 3, 4};
    const auto tail = std::vector{-5, -6, -7};
#ifdef __cpp_lib_containers_ranges
    head.append_range(tail);
#else
    head.insert(head.end(), tail.cbegin(), tail.cend());
#endif
    assert(std::ranges::equal(head, std::list{1, 2, 3, 4, -5, -6, -7}));
}
```

### See also

- **prepend_range (C++23)** — adds a range of elements to the beginning (public
  member function)
- **insert_range (C++23)** — inserts a range of elements (public member
  function)
- **push_back** — adds an element to the end (public member function)
- **emplace_back (C++11)** — constructs an element in-place at the end (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/container/list/append_range*
