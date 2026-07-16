# std::pmr::monotonic_buffer_resource::do_deallocate

```cpp
virtual void do_deallocate( void* p, std::size_t bytes, std::size_t alignment );  // (since C++17)
```

This function has no effect. Memory used by a `monotonic_buffer_resource`, as
its name indicates, increases monotonically until the resource is destroyed.

### Exceptions

Throws nothing.

### See also

- **deallocate** — deallocates memory (public member function of
  `std::pmr::memory_resource`)
- **do_deallocate [virtual]** — deallocates memory (virtual private member
  function of `std::pmr::memory_resource`)

---
*Source: https://en.cppreference.com/w/cpp/memory/monotonic_buffer_resource/do_deallocate*
