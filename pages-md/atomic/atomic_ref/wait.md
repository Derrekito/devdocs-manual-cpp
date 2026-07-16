# std::atomic_ref<T>::wait

```cpp
void wait( T old, std::memory_order order =
                      std::memory_order::seq_cst ) const noexcept;  // (1) (since C++20)
void wait( T old, std::memory_order order =
                      std::memory_order::seq_cst ) const volatile noexcept;  // (2) (since C++20)
```

Performs atomic waiting operations. Behaves as if it repeatedly performs the
following steps:

- Compare the value representation of `this->load(order)` with that of `old`. If
  those are equal, then blocks until `*this` is notified by `notify_one()` or
  `notify_all()`, or the thread is unblocked spuriously. Otherwise, returns.

These functions are guaranteed to return only if value has changed, even if
underlying implementation unblocks spuriously.

If `order` is one of `std::memory_order::release` and
`std::memory_order::acq_rel`, the behavior is undefined.

### Parameters

- **old** — the value to check the `atomic_ref`'s object no longer contains
- **order** — memory order constraints to enforce

### Return value

(none)

### Notes

This form of change-detection is often more efficient than simple polling or
pure spinlocks.

Due to the ABA problem, transient changes from `old` to another value and back
to `old` might be missed, and not unblock.

The comparison is bitwise (similar to `std::memcmp`); no comparison operator is
used. Padding bits that never participate in an object's value representation
are ignored.

### Example

### See also

- **notify_one (C++20)** — notifies at least one thread waiting on the atomic
  object (public member function)
- **notify_all (C++20)** — notifies all threads blocked waiting on the atomic
  object (public member function)
- **atomic_notify_one (C++20)** — notifies a thread blocked in atomic_wait
  (function template)
- **atomic_notify_all (C++20)** — notifies all threads blocked in atomic_wait
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref/wait*
