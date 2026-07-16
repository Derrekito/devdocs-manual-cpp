# std::scoped_allocator_adaptor<OuterAlloc,InnerAlloc...>::allocate

```cpp
pointer allocate( size_type n );  // (since C++11) (until C++20)
[[nodiscard]] pointer allocate( size_type n );  // (since C++20)
pointer allocate( size_type n, const_void_pointer hint );  // (since C++11) (until C++20)
[[nodiscard]] pointer allocate( size_type n, const_void_pointer hint );  // (since C++20)
```

Uses the outer allocator to allocate uninitialized storage.

1) Calls `std::allocator_traits<OuterAlloc>::allocate(outer_allocator(), n)`.

2) Additionally provides memory locality hint, by calling
   `std::allocator_traits<OuterAlloc>::allocate(outer_allocator(), n, hint)`.

### Parameters

- **n** — the number of objects to allocate storage for
- **hint** — pointer to a nearby memory location

### Return value

The pointer to the allocated storage.

### See also

- **allocate** — allocates uninitialized storage (public member function of
  `std::allocator<T>`)
- **allocate [static]** — allocates uninitialized storage using the allocator
  (public static member function of `std::allocator_traits<Alloc>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/scoped_allocator_adaptor/allocate*
