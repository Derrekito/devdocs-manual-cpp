# std::pmr::pool_options

```cpp
struct pool_options;  // (since C++17)
```

`std::pmr::pool_options` is a set of constructor options for pool resources
including `std::pmr::synchronized_pool_resource` and
`std::pmr::unsynchronized_pool_resource`.

### Data members

- **std::size_t max_blocks_per_chunk;** — The maximum number of blocks that will
  be allocated at once from the upstream `std::pmr::memory_resource` to
  replenish the pool. If the value of `max_blocks_per_chunk` is zero or is
  greater than an implementation-defined limit, that limit is used instead. The
  implementation may choose to use a smaller value than is specified in this
  field and may use different values for different pools.
- **std::size_t max_blocks_per_chunk;**
- **std::size_t largest_required_pool_block;** — The largest allocation size
  that is required to be fulfilled using the pooling mechanism. Attempts to
  allocate a single block larger than this threshold will be allocated directly
  from the upstream `std::pmr::memory_resource`. If
  `largest_required_pool_block` is zero or is greater than an
  implementation-defined limit, that limit is used instead. The implementation
  may choose a pass-through threshold larger than specified in this field.
- **std::size_t largest_required_pool_block;**

### See also

- **synchronized_pool_resource (C++17)** — a thread-safe
  `std::pmr::memory_resource` for managing allocations in pools of different
  block sizes (class)
- **unsynchronized_pool_resource (C++17)** — a thread-unsafe
  `std::pmr::memory_resource` for managing allocations in pools of different
  block sizes (class)

---
*Source: https://en.cppreference.com/w/cpp/memory/pool_options*
