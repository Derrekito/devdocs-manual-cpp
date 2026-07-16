# std::atomic_ref<T>::exchange

```cpp
T exchange( T desired, std::memory_order order = std::memory_order_seq_cst ) const noexcept;  // (since C++20)
```

Atomically replaces the value of the referenced object with `desired`. The
operation is a read-modify-write operation. Memory is affected according to the
value of `order`.

### Parameters

- **desired** — value to assign
- **order** — memory order constraints to enforce

### Return value

The value of the referenced object before the call.

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref/exchange*
