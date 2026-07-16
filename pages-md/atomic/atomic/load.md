# std::atomic<T>::load

```cpp
T load( std::memory_order order
            = std::memory_order_seq_cst ) const noexcept;  // (1) (since C++11)
T load( std::memory_order order
            = std::memory_order_seq_cst ) const volatile noexcept;  // (2) (since C++11)
```

Atomically loads and returns the current value of the atomic variable. Memory is
affected according to the value of `order`.

If `order` is one of `std::memory_order_release` and
`std::memory_order_acq_rel`, the behavior is undefined.

It is deprecated if `std::atomic<T>::is_always_lock_free` is `false` and
overload (2) participates in overload resolution.
*(since C++20)*

### Parameters

- **order** — memory order constraints to enforce

### Return value

The current value of the atomic variable.

### See also

- **operator T** — loads a value from an atomic object (public member function)
- **atomic_loadatomic_load_explicit (C++11)(C++11)** — atomically obtains the
  value stored in an atomic object (function template)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/load*
