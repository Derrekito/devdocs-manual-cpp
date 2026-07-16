# std::atomic_ref<T>::fetch_or

```cpp
T fetch_or( T arg,
            std::memory_order order = std::memory_order_seq_cst ) const noexcept;  // (since C++20) (member only of atomic_ref<Integral> template specialization)
```

Atomically replaces the current value of the referenced object with the result
of bitwise OR of the value and `arg`. This operation is a read-modify-write
operation. Memory is affected according to the value of `order`.

### Parameters

- **arg** — the other argument of bitwise OR
- **order** — memory order constraints to enforce

### Return value

The value of the referenced object, immediately preceding the effects of this
function.

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref/fetch_or*
