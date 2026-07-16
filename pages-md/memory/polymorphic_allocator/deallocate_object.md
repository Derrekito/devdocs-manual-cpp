# std::pmr::polymorphic_allocator<T>::deallocate_object

```cpp
template< class U >
void deallocate_object( U* p, std::size_t n = 1 );  // (since C++20)
```

Deallocates the storage pointed to by `p`, which must have been allocated from a
`std::pmr::memory_resource` `x` that compares equal to `*resource()`, using
`x.allocate(n * sizeof(U), alignof(U))`, typically through a call to
`allocate_object<U>(n)`.

Equivalent to `deallocate_bytes(p, n * sizeof(U), alignof(U));`.

### Parameters

- **p** — pointer to memory to deallocate
- **n** — number of objects of type U the memory was for

### Exceptions

Throws nothing.

### Notes

This function was introduced for use with the fully-specialized allocator
`std::pmr::polymorphic_allocator<>`, but it may be useful in any specialization.

### See also

- **deallocate_bytes (C++20)** — free raw memory obtained from `allocate_bytes`
  (public member function)
- **delete_object (C++20)** — destroys and deallocates an object (public member
  function)
- **deallocate [static]** — deallocates storage using the allocator (public
  static member function of `std::allocator_traits<Alloc>`)
- **deallocate** — deallocates memory (public member function of
  `std::pmr::memory_resource`)

---
*Source: https://en.cppreference.com/w/cpp/memory/polymorphic_allocator/deallocate_object*
