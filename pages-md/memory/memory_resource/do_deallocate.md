# std::pmr::memory_resource::do_deallocate

```cpp
virtual void do_deallocate( void* p, std::size_t bytes, std::size_t alignment ) = 0;  // (since C++17)
```

Deallocates the storage pointed to by `p`.

`p` must have been returned by a prior call to `allocate(bytes, alignment)` on a
`memory_resource` that compares equal to `*this`, and the storage it points to
must not yet have been deallocated, otherwise the behavior is undefined.

### Exceptions

Throws nothing.

### See also

- **deallocate** — deallocates memory (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/memory_resource/do_deallocate*
