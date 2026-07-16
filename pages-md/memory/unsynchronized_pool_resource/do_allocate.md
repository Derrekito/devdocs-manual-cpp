# std::pmr::unsynchronized_pool_resource::do_allocate

```cpp
virtual void* do_allocate( std::size_t bytes, std::size_t alignment );  // (since C++17)
```

Allocates storage.

If the pool selected for a block of size `bytes` is unable to satisfy the
request from its internal data structures, calls `allocate()` on the upstream
memory resource to obtain memory.

If the size requested is larger than what the largest pool can handle, memory is
allocated by calling `allocate()` on the upstream memory resource.

### Return value

A pointer to allocated storage of at least `bytes` bytes in size, aligned to the
specified `alignment` if such alignment is supported, and to
`alignof(std::max_align_t)` otherwise.

### Exceptions

Throws nothing unless calling `allocate()` on the upstream memory resource
throws.

### See also

- **allocate** — allocates memory (public member function of
  `std::pmr::memory_resource`)
- **do_allocate [virtual]** — allocates memory (virtual private member function
  of `std::pmr::memory_resource`)

---
*Source: https://en.cppreference.com/w/cpp/memory/unsynchronized_pool_resource/do_allocate*
