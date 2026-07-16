# std::pmr::synchronized_pool_resource::upstream_resource

```cpp
std::pmr::memory_resource* upstream_resource() const;  // (since C++17)
```

Returns a pointer to the upstream memory resource. This is the same value as the
`upstream` argument passed to the constructor of this object.

### See also

- **(constructor)** — constructs a `synchronized_pool_resource` (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/memory/synchronized_pool_resource/upstream_resource*
