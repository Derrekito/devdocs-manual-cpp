# std::pmr::polymorphic_allocator<T>:: select_on_container_copy_construction

```cpp
polymorphic_allocator select_on_container_copy_construction() const;  // (since C++17)
```

Returns a default-constructed `polymorphic_allocator` object.

### Parameters

(none)

### Return value

A default-constructed `polymorphic_allocator` object.

### Notes

`polymorphic_allocator`s do not propagate on container copy construction.

### See also

- **select_on_container_copy_construction [static]** — obtains the allocator to
  use after copying a standard container (public static member function of
  `std::allocator_traits<Alloc>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/polymorphic_allocator/select_on_container_copy_construction*
