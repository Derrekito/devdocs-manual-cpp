# std::pmr::memory_resource::deallocate

```cpp
void deallocate( void* p,
                 std::size_t bytes,
                 std::size_t alignment = alignof(std::max_align_t) );  // (since C++17)
```

Deallocates the storage pointed to by `p`. `p` shall have been returned by a
prior call to `allocate(bytes, alignment)` on a `memory_resource` that compares
equal to `*this`, and the storage it points to shall not yet have been
deallocated.

Equivalent to `do_deallocate(p, bytes, alignment);`.

### Exceptions

Throws nothing.

### See also

- **do_deallocate [virtual]** — deallocates memory (virtual private member
  function)

---
*Source: https://en.cppreference.com/w/cpp/memory/memory_resource/deallocate*
