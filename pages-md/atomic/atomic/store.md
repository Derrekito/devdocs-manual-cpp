# std::atomic<T>::store

```cpp
void store( T desired, std::memory_order order =
                           std::memory_order_seq_cst ) noexcept;  // (1) (since C++11)
void store( T desired, std::memory_order order =
                           std::memory_order_seq_cst ) volatile noexcept;  // (2) (since C++11)
```

Atomically replaces the current value with `desired`. Memory is affected
according to the value of `order`.

If `order` is one of `std::memory_order_consume`, `std::memory_order_acquire`
and `std::memory_order_acq_rel`, the behavior is undefined.

It is deprecated if `std::atomic<T>::is_always_lock_free` is `false` and
overload (2) participates in overload resolution.
*(since C++20)*

### Parameters

- **desired** — the value to store into the atomic variable
- **order** — memory order constraints to enforce

### Return value

(none)

### See also

- **operator=** — stores a value into an atomic object (public member function)
- **atomic_storeatomic_store_explicit (C++11)(C++11)** — atomically replaces the
  value of the atomic object with a non-atomic argument (function template)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/store*
