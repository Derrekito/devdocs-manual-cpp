# std::deque<T,Allocator>::append_range

```cpp
template< container-compatible-range<T> R >
void append_range( R&& rg );  // (since C++23)
```

Inserts copies of elements from the range `rg` before `end()`, in non-reversing
order.

All iterators (including the `end()` iterator) are invalidated. No references
are invalidated.

Each iterator in `rg` is dereferenced exactly once.

### Parameters

- **rg** — a container compatible range, that is, an `input_range` whose
  elements are convertible to `T`

**Type requirements**

**-`T` must be EmplaceConstructible into `deque` from `*ranges::begin(rg)`. Otherwise, the behavior is undefined.**

### Return value

(none)

### Complexity

Linear in size of `rg`. The number of calls to the constructor of `T` is exactly
equal to the `std::ranges::size(rg))`.

### Exceptions

If an exception is thrown other than by the copy constructor, move constructor,
assignment operator, or move assignment operator of `T` there are no effects. If
an exception is thrown while inserting a single element at either end, there are
no effects. Otherwise, if an exception is thrown by the move constructor of a
non-CopyInsertable `T`, the effects are unspecified.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges-aware construction
      and insertion

### Example

```cpp
#include <algorithm>
#include <cassert>
#include <deque>
#include <list>

int main()
{
    auto head = std::deque{1, 2, 3, 4};
    const auto tail = std::list{-5, -6, -7};
#ifdef __cpp_lib_containers_ranges
    head.append_range(tail);
#else
    head.insert(head.end(), tail.cbegin(), tail.cend());
#endif
    assert(std::ranges::equal(head, std::deque{1, 2, 3, 4, -5, -6, -7}));
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
*Source: https://en.cppreference.com/w/cpp/container/deque/append_range*
