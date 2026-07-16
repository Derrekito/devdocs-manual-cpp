# std::atomic_ref<T>::store

```cpp
void store( T desired, std::memory_order order = std::memory_order_seq_cst ) const noexcept;  // (since C++20)
```

Atomically replaces the current value of the referenced object with `desired`.
Memory is affected according to the value of `order`.

`order` must be one of `std::memory_order_relaxed`, `std::memory_order_release`
or `std::memory_order_seq_cst`. Otherwise the behavior is undefined.

### Parameters

- **desired** — the value to store into the referenced object
- **order** — memory order constraints to enforce

### Return value

(none)

### See also

- **operator=** — stores a value into the object referenced by an `atomic_ref`
  object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref/store*
