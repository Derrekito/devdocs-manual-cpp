# std::pmr::synchronized_pool_resource::synchronized_pool_resource

```cpp
synchronized_pool_resource();  // (1) (since C++17)
explicit synchronized_pool_resource( std::pmr::memory_resource* upstream );  // (2) (since C++17)
explicit synchronized_pool_resource( const std::pmr::pool_options& opts );  // (3) (since C++17)
synchronized_pool_resource( const std::pmr::pool_options& opts,
                            std::pmr::memory_resource* upstream );  // (4) (since C++17)
synchronized_pool_resource( const synchronized_pool_resource& ) = delete;  // (5) (since C++17)
```

Constructs a `synchronized_pool_resource`.

1-4) Constructs a `synchronized_pool_resource` using the specified upstream
   memory resource and tuned according to the specified options. The resulting
   object holds a copy of `upstream` but does not own the resource to which
   `upstream` points. The overloads not taking `opts` as a parameter uses a
   default constructed instance of `pool_options` as the options. The overloads
   not taking `upstream` as a parameter uses the return value of
   `std::pmr::get_default_resource` as the upstream memory resource.

5) Copy constructor is deleted.

### Parameters

- **opts** — a `std::pmr::pool_options` struct containing the constructor
  options
- **upstream** — the upstream memory resource to use

### Exceptions

1-4) Throws only if a call to the `allocate()` function of the upstream resource
   throws. It is unspecified if or under what conditions such a call takes
   place.

---
*Source: https://en.cppreference.com/w/cpp/memory/synchronized_pool_resource/synchronized_pool_resource*
