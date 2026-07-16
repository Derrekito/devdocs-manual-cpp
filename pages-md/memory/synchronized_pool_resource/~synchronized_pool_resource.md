# std::pmr::synchronized_pool_resource::~synchronized_pool_resource

```cpp
virtual ~synchronized_pool_resource();  // (since C++17)
```

Destroys a `synchronized_pool_resource`.

Deallocates all memory owned by this resource by calling `this->release()`.

### See also

- **release** — release all allocated memory (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/synchronized_pool_resource/%7Esynchronized_pool_resource*
