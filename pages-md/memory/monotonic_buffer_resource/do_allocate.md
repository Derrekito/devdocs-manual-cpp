# std::pmr::monotonic_buffer_resource::do_allocate

```cpp
virtual void* do_allocate( std::size_t bytes, std::size_t alignment );  // (since C++17)
```

Allocates storage.

If the *current buffer* has sufficient unused space to fit a block with the
specified size and alignment, allocates the return block from the current
buffer.

Otherwise, this function allocates a new buffer by calling
`upstream_resource()->allocate(n, m)`, where `n` is not less than the greater of
`bytes` and the *next buffer size* and `m` is not less than `alignment`. It sets
the new buffer as the *current buffer*, increases the *next buffer size* by an
implementation-defined growth factor (which is not necessarily integral), and
then allocates the return block from the newly allocated buffer.

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
*Source: https://en.cppreference.com/w/cpp/memory/monotonic_buffer_resource/do_allocate*
