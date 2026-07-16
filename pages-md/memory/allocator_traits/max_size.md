# std::allocator_traits<Alloc>::max_size

```cpp
static size_type max_size( const Alloc& a ) noexcept;  // (since C++11) (until C++20)
static constexpr size_type max_size( const Alloc& a ) noexcept;  // (since C++20)
```

If possible, obtains the maximum theoretically possible allocation size from the
allocator `a`, by calling `a.max_size()`.

If the above is not possible (e.g. `Alloc` does not have the member function
`max_size()`), then returns `std::numeric_limits<size_type>::max() /
sizeof(value_type)`.

### Parameters

- **a** — allocator to detect

### Return value

Theoretical maximum allocation size.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2162 | C++11 | `max_size` was not requied to be noexcept | required
  LWG 2466 | C++11 | theoretical maximum allocation size in bytes was returned
      as fallback | size in elements is returned

### See also

- **max_size (until C++20)** — returns the largest supported allocation size
  (public member function of `std::allocator<T>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator_traits/max_size*
