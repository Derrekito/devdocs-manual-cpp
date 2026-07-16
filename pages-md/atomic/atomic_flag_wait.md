# std::atomic_flag_wait, std::atomic_flag_wait_explicit

```cpp
void atomic_flag_wait( const atomic_flag* object, bool old ) noexcept;  // (1) (since C++20)
void atomic_flag_wait( const volatile atomic_flag* object,
                       bool old ) noexcept;  // (2) (since C++20)
void atomic_flag_wait_explicit( const atomic_flag* object,
                                bool old, std::memory_order order ) noexcept;  // (3) (since C++20)
void atomic_flag_wait_explicit( const volatile atomic_flag* object,
                                bool old, std::memory_order order ) noexcept;  // (4) (since C++20)
```

Performs atomic waiting operations.

Compares `object->test(std::memory_order_seq_cst)` or `object->test(order)` with
`old`, and if they are equal then blocks until `*object` is notified by
`std::atomic_flag::notify_one()` or `std::atomic_flag::notify_all()` (or the
thread is unblocked spuriously). This is repeated until the values compare
unequal.

1,2) The memory synchronization order is `std::memory_order_seq_cst`.

3,4) The memory synchronization order is `order`.

If `order` is one of `std::memory_order_release` and
   `std::memory_order_acq_rel`, the behavior is undefined.

These functions are guaranteed to return only if value has changed, even if
underlying implementation unblocks spuriously.

### Parameters

- **object** — pointer to the atomic flag to check and wait on
- **old** — the value to check the atomic flag no longer contains
- **order** — the memory synchronization ordering

### Return value

(none)

### Notes

This form of change-detection is often more efficient than simple polling or
pure spinlocks.

Due to the ABA problem, transient changes from `old` to another value and back
to `old` might be missed, and not unblock.

### Example

### See also

- **notify_one (C++20)** — notifies at least one thread waiting on the atomic
  object (public member function of `std::atomic_flag`)
- **notify_all (C++20)** — notifies all threads blocked waiting on the atomic
  object (public member function of `std::atomic_flag`)
- **atomic_flag_notify_one (C++20)** — notifies a thread blocked in
  atomic_flag_wait (function)
- **atomic_flag_notify_all (C++20)** — notifies all threads blocked in
  atomic_flag_wait (function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_flag_wait*
