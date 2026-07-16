# std::pmr::synchronized_pool_resource::do_deallocate

```cpp
virtual void do_deallocate( void* p, std::size_t bytes, std::size_t alignment );  // (since C++17)
```

Returns the memory at `p` to the pool. It is unspecified if or under what
circumstances this operation will result in a call to `deallocate()` on the
upstream memory resource.

### Exceptions

Throws nothing.

### See also

- **deallocate** — deallocates memory (public member function of
  `std::pmr::memory_resource`)
- **do_deallocate [virtual]** — deallocates memory (virtual private member
  function of `std::pmr::memory_resource`)

---
*Source: https://en.cppreference.com/w/cpp/memory/synchronized_pool_resource/do_deallocate*
