# std::vector<T,Allocator>::insert_range

```cpp
template< container-compatible-range<T> R >
constexpr iterator insert_range( const_iterator pos, R&& rg );  // (since C++23)
```

Inserts, in non-reversing order, copies of elements in `rg` before `pos`.

If after the operation the new `size()` is greater than old `capacity()` a
reallocation takes place, in which case all iterators (including the `end()`
iterator) and all references to the elements are invalidated. Otherwise, only
the iterators and references before the insertion point remain valid.

Each iterator in the range `rg` is dereferenced exactly once.

The behavior is undefined if `rg` overlaps with the container.

### Parameters

- **pos** — iterator before which the content will be inserted (`pos` may be the
  `end()` iterator)
- **rg** — a container compatible range, that is, an `input_range` whose
  elements are convertible to `T`

**Type requirements**

**-`T` must be EmplaceConstructible into `vector` from `*ranges::begin(rg)`. Also, `T` must be MoveInsertable into `vector` and `T` satisfies MoveConstructible, MoveAssignable, and Swappable. Otherwise, the behavior is undefined.**

### Return value

An `iterator` that points at the copy of the first element inserted into
`vector` or at `pos` if `rg` is empty.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges-aware construction
      and insertion

### Example

```cpp
#include <algorithm>
#include <cassert>
#include <iterator>
#include <vector>
#include <list>

int main()
{
    auto container = std::vector{1, 2, 3, 4};
    auto pos = std::next(container.begin(), 2);
    assert(*pos == 3);
    const auto rg = std::list{-1, -2, -3};

#ifdef __cpp_lib_containers_ranges
    container.insert_range(pos, rg);
#else
    container.insert(pos, rg.cbegin(), rg.cend());
#endif
    assert(std::ranges::equal(container, std::vector{1, 2, -1, -2, -3, 3, 4}));
}
```

### See also

- **insert** — inserts elements (public member function)
- **append_range (C++23)** — adds a range of elements to the end (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/insert_range*
