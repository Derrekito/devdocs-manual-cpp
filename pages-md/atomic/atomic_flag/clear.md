# std::atomic_flag::clear

```cpp
void clear( std::memory_order order =
                std::memory_order_seq_cst ) volatile noexcept;  // (1) (since C++11)
void clear( std::memory_order order =
                std::memory_order_seq_cst ) noexcept;  // (2) (since C++11)
```

Atomically changes the state of a `std::atomic_flag` to clear (`false`).

If `order` is one of `std::memory_order_consume`, `std::memory_order_acquire`
and `std::memory_order_acq_rel`, the behavior is undefined.

### Parameters

- **order** — the memory synchronization ordering

### See also

- **test_and_set** — atomically sets the flag to `true` and obtains its previous
  value (public member function)
- **atomic_flag_clearatomic_flag_clear_explicit (C++11)(C++11)** — atomically
  sets the value of the flag to `false` (function)
- **memory_order (C++11)** — defines memory ordering constraints for the given
  atomic operation (enum)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_flag/clear*
