# std::pmr::polymorphic_allocator<T>::allocate_object

```cpp
template< class U >
[[nodiscard]] U* allocate_object( std::size_t n = 1 );  // (since C++20)
```

Allocates storage for `n` objects of type `U` using the underlying memory
resource.

If `std::numeric_limits<std::size_t>::max() / sizeof(U) < n`, throws
`std::bad_array_new_length`, otherwise equivalent to `return
static_cast<U*>(allocate_bytes(n * sizeof(U), alignof(U)));`.

### Parameters

- **n** — the number of objects to allocate storage for

### Return value

A pointer to the allocated storage.

### Notes

This function was introduced for use with the fully-specialized allocator
`std::pmr::polymorphic_allocator<>`, but it may be useful in any specialization
as a shortcut to avoid having to rebind from
`std::pmr::polymorphic_allocator<T>` to `std::pmr::polymorphic_allocator<U>`.

Since `U` is not deduced, it must be provided as a template argument when
calling this function.

### Exceptions

Throws `std::bad_array_new_length` if `n >
std::numeric_limits<std::size_t>::max() / sizeof(U)`; may also be any exceptions
thrown by the call to `resource()->allocate`.

### See also

- **allocate_bytes (C++20)** — allocate raw aligned memory from the underlying
  resource (public member function)
- **new_object (C++20)** — allocates and constructs an object (public member
  function)
- **allocate** — allocate memory (public member function)
- **allocate [static]** — allocates uninitialized storage using the allocator
  (public static member function of `std::allocator_traits<Alloc>`)
- **allocate** — allocates memory (public member function of
  `std::pmr::memory_resource`)

---
*Source: https://en.cppreference.com/w/cpp/memory/polymorphic_allocator/allocate_object*
