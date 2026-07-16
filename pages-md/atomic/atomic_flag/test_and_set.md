# std::atomic_flag::test_and_set

```cpp
bool test_and_set( std::memory_order order =
                       std::memory_order_seq_cst ) volatile noexcept;  // (1) (since C++11)
bool test_and_set( std::memory_order order =
                       std::memory_order_seq_cst ) noexcept;  // (2) (since C++11)
```

Atomically changes the state of a `std::atomic_flag` to set (`true`) and returns
the value it held before.

### Parameters

- **order** — the memory synchronization order

### See also

- **clear** — atomically sets flag to `false` (public member function)
- **atomic_flag_test_and_setatomic_flag_test_and_set_explicit (C++11)(C++11)** —
  atomically sets the flag to `true` and returns its previous value (function)
- **memory_order (C++11)** — defines memory ordering constraints for the given
  atomic operation (enum)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_flag/test_and_set*
