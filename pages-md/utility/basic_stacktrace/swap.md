# std::basic_stacktrace<Allocator>::swap

```cpp
void swap( basic_stacktrace& other ) noexcept(/* see below */);  // (since C++23)
```

Exchanges the contents of the container with those of `other`. Does not invoke
any move, copy, or swap operations on individual `stacktrace_entry` objects.

All iterators and references remain valid. The `end()` iterator is invalidated.

If `std::allocator_traits<allocator_type>::propagate_on_container_swap::value`
is true, then the allocators are exchanged using an unqualified call to
non-member `swap`. Otherwise, they are not swapped (and if `get_allocator() !=
other.get_allocator()`, the behavior is undefined).

### Parameters

- **other** — `basic_stacktrace` to exchange the contents with

### Return value

(none)

### Exceptions

`noexcept` specification:

`noexcept(std::allocator_traits<Allocator>::propagate_on_container_swap::value
|| std::allocator_traits<Allocator>::is_always_equal::value)`

### Complexity

Constant.

### Example

### See also

- **std::swap(std::basic_stacktrace) (C++23)** — specializes the `std::swap`
  algorithm (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/basic_stacktrace/swap*
