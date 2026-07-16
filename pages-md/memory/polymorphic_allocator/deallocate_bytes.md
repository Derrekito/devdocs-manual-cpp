# std::pmr::polymorphic_allocator<T>::deallocate_bytes

```cpp
void deallocate_bytes( void* p,
                       std::size_t nbytes,
                       std::size_t alignment = alignof(std::max_align_t) );  // (since C++20)
```

Deallocates the storage pointed to by `p`, which must have been allocated from a
`std::pmr::memory_resource` `x` that compares equal to `*resource()`, using
`x.allocate(nbytes, alignment)`, typically through a call to
`allocate_bytes(nbytes, alignment)`.

Equivalent to `resource()->deallocate(p, nbytes, alignment);`.

### Parameters

- **p** — pointer to memory to deallocate
- **nbytes** — the number of bytes originally allocated
- **alignment** — the alignment originally allocated

### Exceptions

Throws nothing.

### Notes

This function was introduced for use with the fully-specialized allocator
`std::pmr::polymorphic_allocator<>`, but it may be useful in any specialization.

### See also

- **deallocate_object (C++20)** — frees raw memory obtained by `allocate_object`
  (public member function)
- **delete_object (C++20)** — destroys and deallocates an object (public member
  function)
- **deallocate [static]** — deallocates storage using the allocator (public
  static member function of `std::allocator_traits<Alloc>`)
- **deallocate** — deallocates memory (public member function of
  `std::pmr::memory_resource`)

---
*Source: https://en.cppreference.com/w/cpp/memory/polymorphic_allocator/deallocate_bytes*
