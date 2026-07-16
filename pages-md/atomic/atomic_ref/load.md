# std::atomic_ref<T>::load

```cpp
T load( std::memory_order order = std::memory_order_seq_cst ) const noexcept;  // (since C++20)
```

Atomically loads and returns the current value of the referenced object. Memory
is affected according to the value of `order`.

`order` must be one of `std::memory_order_relaxed`, `std::memory_order_consume`,
`std::memory_order_acquire` or `std::memory_order_seq_cst`. Otherwise the
behavior is undefined.

### Parameters

- **order** — memory order constraints to enforce

### Return value

The current value of the referenced object.

### See also

- **operator T** — loads a value from the referenced object (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref/load*
