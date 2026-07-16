# std::vector<T,Allocator>::append_range

```cpp
template< container-compatible-range<T> R >
constexpr void append_range( R&& rg );  // (since C++23)
```

Inserts copies of elements from the range `rg` before `end()`, in non-reversing
order.

If after the operation the new `size()` is greater than old `capacity()` a
reallocation takes place, in which case all iterators (including the `end()`
iterator) and all references to the elements are invalidated. Otherwise only the
`end()` iterator is invalidated.

Each iterator in `rg` is dereferenced exactly once.

### Parameters

- **rg** — a container compatible range, that is, an `input_range` whose
  elements are convertible to `T`

**Type requirements**

**-`T` must be EmplaceConstructible into `vector` from `*ranges::begin(rg)`. Also, `T` must be MoveInsertable into `vector`. Otherwise, the behavior is undefined.**

### Return value

(none)

### Complexity

If reallocation happens, linear in the number of elements of the resulting
`vector`; otherwise, linear in the number of elements inserted plus the distance
to the `end()`.

### Exceptions

If an exception is thrown other than by the copy constructor, move constructor,
assignment operator, or move assignment operator of `T` or by any
`InputIterator` operation there are no effects. If an exception is thrown while
inserting a single element at the end and `T` is CopyInsertable or
`std::is_nothrow_move_constructible_v``<T>` is `true`, there are no effects.
Otherwise, if an exception is thrown by the move constructor of a
non-CopyInsertable `T`, the effects are unspecified.

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
    auto head = std::vector{1, 2, 3, 4};
    const auto tail = std::list{-5, -6, -7};
#ifdef __cpp_lib_containers_ranges
    head.append_range(tail);
#else
    head.insert(head.end(), tail.cbegin(), tail.cend());
#endif
    assert(std::ranges::equal(head, std::vector{1, 2, 3, 4, -5, -6, -7}));
}
```

### See also

- **insert_range (C++23)** — inserts a range of elements (public member
  function)
- **push_back** — adds an element to the end (public member function)
- **emplace_back (C++11)** — constructs an element in-place at the end (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/append_range*
