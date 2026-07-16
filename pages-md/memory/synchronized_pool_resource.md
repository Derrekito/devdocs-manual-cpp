# std::pmr::synchronized_pool_resource

```cpp
class synchronized_pool_resource : public std::pmr::memory_resource;  // (since C++17)
```

The class `std::pmr::synchronized_pool_resource` is a general-purpose memory
resource class with the following properties:

- It owns the allocated memory and frees it on destruction, even if `deallocate`
  has not been called for some of the allocated blocks.
- It consists of a collection of *pools* that serves requests for different
  block sizes. Each pool manages a collection of *chunks* that are then divided
  into blocks of uniform size.
- Calls to `do_allocate` are dispatched to the pool serving the smallest blocks
  accommodating the requested size.
- Exhausting memory in the pool causes the next allocation request for that pool
  to allocate an additional chunk of memory from the *upstream allocator* to
  replenish the pool. The chunk size obtained increases geometrically.
- Allocations requests that exceed the largest block size are served from the
  *upstream allocator* directly.
- The largest block size and maximum chunk size may be tuned by passing a
  `std::pmr::pool_options` struct to its constructor.

`synchronized_pool_resource` may be accessed from multiple threads without
external synchronization, and may have thread-specific pools to reduce
synchronization costs. If the memory resource is only accessed from one thread,
`unsynchronized_pool_resource` is more efficient.

### Member functions

- **(constructor)** — constructs a `synchronized_pool_resource` (public member
  function)
- **(destructor) [virtual]** — destroys a `synchronized_pool_resource`,
  releasing all allocated memory (virtual public member function)
- **operator= [deleted]** — copy assignment operator is deleted.
  `synchronized_pool_resource` is not copy assignable (public member function)

**Public member functions**

- **release** — release all allocated memory (public member function)
- **upstream_resource** — returns a pointer to the upstream memory resource
  (public member function)
- **options** — returns the options that control the pooling behavior of this
  resource (public member function)

**Protected member functions**

- **do_allocate [virtual]** — allocate memory (virtual protected member
  function)
- **do_deallocate [virtual]** — return memory to the pool (virtual protected
  member function)
- **do_is_equal [virtual]** — compare for equality with another
  `memory_resource` (virtual protected member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/synchronized_pool_resource*
