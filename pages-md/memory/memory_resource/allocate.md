# std::pmr::memory_resource::allocate

```cpp
void* allocate( std::size_t bytes,
                std::size_t alignment = alignof(std::max_align_t) );  // (since C++17) (until C++20)
[[nodiscard]] void* allocate( std::size_t bytes,
                              std::size_t alignment = alignof(std::max_align_t) );  // (since C++20)
```

Allocates storage with a size of at least `bytes` bytes, aligned to the
specified `alignment`.

Equivalent to `return do_allocate(bytes, alignment);`.

### Exceptions

Throws an exception if storage of the requested size and alignment cannot be
obtained.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2843 | ??? | over-alignment was allowed to be unsupported | alignment must
      be honoured

### See also

- **do_allocate [virtual]** — allocates memory (virtual private member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/memory_resource/allocate*
