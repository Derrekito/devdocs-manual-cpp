# std::pmr::synchronized_pool_resource::release

```cpp
void release();  // (since C++17)
```

Releases all memory owned by this resource by calling the `deallocate` function
of the upstream memory resource as needed.

Memory is released back to the upstream resource even if `deallocate` has not
been called for some of the allocated blocks.

### See also

- **deallocate** — deallocates memory (public member function of
  `std::pmr::memory_resource`)

---
*Source: https://en.cppreference.com/w/cpp/memory/synchronized_pool_resource/release*
