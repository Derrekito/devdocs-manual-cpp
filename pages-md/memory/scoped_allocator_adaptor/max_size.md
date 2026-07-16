# std::scoped_allocator_adaptor<OuterAlloc,InnerAlloc...>::max_size

```cpp
size_type max_size() const;  // (since C++11)
```

Reports the maximum allocation size supported by the outer allocator, by calling
`std::allocator_traits<OuterAlloc>::max_size(outer_allocator())`.

### Parameters

(none)

### Return value

The maximum allocation size for OuterAlloc.

### See also

- **max_size (until C++20)** — returns the largest supported allocation size
  (public member function of `std::allocator<T>`)
- **max_size [static]** — returns the maximum object size supported by the
  allocator (public static member function of `std::allocator_traits<Alloc>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/scoped_allocator_adaptor/max_size*
