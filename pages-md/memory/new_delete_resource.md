# std::pmr::new_delete_resource

```cpp
std::pmr::memory_resource* new_delete_resource() noexcept;  // (since C++17)
```

Returns a pointer to a `memory_resource` that uses the global `operator new` and
`operator delete` to allocate memory.

### Return value

Returns a pointer `p` to a static storage duration object of a type derived from
`std::pmr::memory_resource`, with the following properties:

- its `allocate()` function uses `::operator new` to allocate memory;
- its `deallocate()` function uses `::operator delete` to deallocate memory;
- for any `memory_resource` `r`, `p->is_equal(r)` returns `&r == p`.

The same value is returned every time this function is called.

---
*Source: https://en.cppreference.com/w/cpp/memory/new_delete_resource*
