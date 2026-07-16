# std::pmr::polymorphic_allocator<T>::allocate_bytes

```cpp
[[nodiscard]] void* allocate_bytes( std::size_t nbytes,
                                    std::size_t alignment = alignof(std::max_align_t) );  // (since C++20)
```

Allocates `nbytes` bytes of storage at specified alignment `alignment` using the
underlying memory resource. Equivalent to `return resource()->allocate(nbytes,
alignment);`.

### Parameters

- **nbytes** — the number of bytes to allocate
- **alignment** — the alignment to use

### Return value

A pointer to the allocated storage.

### Notes

This function was introduced for use with the fully-specialized allocator
`std::pmr::polymorphic_allocator<>`, but it may be useful in any specialization.

The return type is `void*` (rather than, e.g., `std::byte*`) to support
conversion to an arbitrary pointer type `U*` by `static_cast<U*>`.

### Exceptions

May throw any exceptions thrown by the call to `resource()->allocate`.

### See also

- **allocate_object (C++20)** — allocates raw memory suitable for an object or
  an array (public member function)
- **new_object (C++20)** — allocates and constructs an object (public member
  function)
- **allocate** — allocate memory (public member function)
- **allocate [static]** — allocates uninitialized storage using the allocator
  (public static member function of `std::allocator_traits<Alloc>`)
- **allocate** — allocates memory (public member function of
  `std::pmr::memory_resource`)

---
*Source: https://en.cppreference.com/w/cpp/memory/polymorphic_allocator/allocate_bytes*
