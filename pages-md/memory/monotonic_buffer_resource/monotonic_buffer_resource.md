# std::pmr::monotonic_buffer_resource::monotonic_buffer_resource

```cpp
monotonic_buffer_resource();  // (1) (since C++17)
explicit monotonic_buffer_resource( std::pmr::memory_resource* upstream );  // (2) (since C++17)
explicit monotonic_buffer_resource( std::size_t initial_size );  // (3) (since C++17)
monotonic_buffer_resource( std::size_t initial_size,
                           std::pmr::memory_resource* upstream );  // (4) (since C++17)
monotonic_buffer_resource( void* buffer, std::size_t buffer_size );  // (5) (since C++17)
monotonic_buffer_resource( void* buffer, std::size_t buffer_size,
                           std::pmr::memory_resource* upstream );  // (6) (since C++17)
monotonic_buffer_resource( const monotonic_buffer_resource& ) = delete;  // (7) (since C++17)
```

Constructs a `monotonic_buffer_resource`. The constructors not taking an
upstream memory resource pointer use the return value of
`std::pmr::get_default_resource` as the upstream memory resource.

1,2) Sets the *current buffer* to null and the *next buffer size* to an
   implementation-defined size.

3,4) Sets the *current buffer* to null and the *next buffer size* to a size no
   smaller than `initial_size`.

5,6) Sets the *current buffer* to `buffer` and the *next buffer size* to
   `buffer_size` (but not less than 1). Then increase the *next buffer size* by
   an implementation-defined growth factor (which does not have to be integral).

7) Copy constructor is deleted.

### Parameters

- **upstream** — the upstream memory resource to use; must point to a valid
  memory resource
- **initial_size** — the minimum size of the first buffer to allocate; must be
  greater than zero
- **buffer** — the initial buffer to use
- **buffer_size** — the size of the initial buffer; cannot be greater than the
  number of bytes in `buffer`

---
*Source: https://en.cppreference.com/w/cpp/memory/monotonic_buffer_resource/monotonic_buffer_resource*
