# std::scoped_allocator_adaptor<OuterAlloc,InnerAlloc...>::deallocate

```cpp
void deallocate( pointer p, size_type n ) noexcept;  // (since C++11)
```

Uses the outer allocator to deallocate the storage referenced by `p`, by calling
`std::allocator_traits<OuterAlloc>::deallocate(outer_allocator(), p, n)`.

### Parameters

- **p** — pointer to the previously allocated memory
- **n** — the number of objects for which the memory was allocated

### Return value

(none)

### See also

- **deallocate** — deallocates storage (public member function of
  `std::allocator<T>`)
- **deallocate [static]** — deallocates storage using the allocator (public
  static member function of `std::allocator_traits<Alloc>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/scoped_allocator_adaptor/deallocate*
