# std::pmr::memory_resource::do_allocate

```cpp
virtual void* do_allocate( std::size_t bytes, std::size_t alignment ) = 0;  // (since C++17)
```

Allocates storage with a size of at least `bytes` bytes, aligned to the
specified `alignment`.

`alignment` shall be a power of two.

### Exceptions

Throws an exception if storage of the requested size and alignment cannot be
obtained.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2843 | C++17 | handling of unsupported alignment contradictory | throws an
      exception

### See also

- **allocate** — allocates memory (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/memory_resource/do_allocate*
