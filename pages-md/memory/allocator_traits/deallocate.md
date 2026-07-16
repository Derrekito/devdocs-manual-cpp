# std::allocator_traits<Alloc>::deallocate

```cpp
static void deallocate( Alloc& a, pointer p, size_type n );  // (since C++11) (until C++20)
static constexpr void deallocate( Alloc& a, pointer p, size_type n );  // (since C++20)
```

Uses the allocator `a` to deallocate the storage referenced by `p`, by calling
`a.deallocate(p, n)`.

### Parameters

- **a** — allocator to use
- **p** — pointer to the previously allocated storage
- **n** — the number of objects the storage was allocated for

### Return value

(none)

### Example

### See also

- **allocate [static]** — allocates uninitialized storage using the allocator
  (public static member function)
- **deallocate** — deallocates storage (public member function of
  `std::allocator<T>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator_traits/deallocate*
