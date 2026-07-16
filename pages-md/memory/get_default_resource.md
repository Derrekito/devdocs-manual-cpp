# std::pmr::get_default_resource

```cpp
std::pmr::memory_resource* get_default_resource() noexcept;  // (since C++17)
```

Gets the default memory resource pointer.

The *default memory resource pointer* is used by certain facilities when an
explicit memory resource is not supplied. The initial default memory resource
pointer is the return value of `std::pmr::new_delete_resource`.

This function is thread-safe. Previous call to `std::pmr::set_default_resource`
*synchronizes with* (see `std::memory_order`) the subsequent
`std::pmr::get_default_resource` calls.

### Return value

Returns the value of the default memory resource pointer.

### See also

- **set_default_resource (C++17)** — sets the default
  `std::pmr::memory_resource` (function)
- **new_delete_resource (C++17)** — returns a static program-wide
  `std::pmr::memory_resource` that uses the global `operator new` and `operator
  delete` to allocate and deallocate memory (function)

---
*Source: https://en.cppreference.com/w/cpp/memory/get_default_resource*
