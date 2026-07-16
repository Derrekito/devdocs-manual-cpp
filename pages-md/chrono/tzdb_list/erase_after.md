# std::chrono::tzdb_list::erase_after

```cpp
const_iterator erase_after( const_iterator p );  // (since C++20)
```

Erases the `std::chrono::tzdb` referred to by the iterator following `p`. The
behavior is undefined if that iterator is not dereferenceable. No pointers,
references, or iterators are invalidated except for those referring to the
erased element.

### Return value

An iterator pointing to the element following the erased element, or `end()` if
no such element exists.

### Notes

`tzdb_list` is intended to be implementable as a singly linked list, and its
interface resembles that of `std::forward_list`. It has no `before_begin()`,
however, and so it is not possible to erase the first element.

---
*Source: https://en.cppreference.com/w/cpp/chrono/tzdb_list/erase_after*
