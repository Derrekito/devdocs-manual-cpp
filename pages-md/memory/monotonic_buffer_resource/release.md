# std::pmr::monotonic_buffer_resource::release

```cpp
void release();  // (since C++17)
```

Releases all allocated memory by calling the `deallocate` function on the
upstream memory resource as necessary. Resets *current buffer* and *next buffer
size* to their initial values at construction.

Memory is released back to the upstream resource even if `deallocate` has not
been called for some of the allocated blocks.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3120 | C++17 | `release` might not make initial buffer reusable if
      provided | required to do so

### See also

- **deallocate** — deallocates memory (public member function of
  `std::pmr::memory_resource`)

---
*Source: https://en.cppreference.com/w/cpp/memory/monotonic_buffer_resource/release*
