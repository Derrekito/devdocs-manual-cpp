# std::atomic_flag_notify_one

```cpp
void atomic_flag_notify_one( std::atomic_flag* object ) noexcept;  // (1) (since C++20)
void atomic_flag_notify_one( volatile std::atomic_flag* object ) noexcept;  // (2) (since C++20)
```

Performs atomic notifying operations.

If there is a thread blocked in an atomic waiting operation (i.e.
`std::atomic_flag_wait()`, `std::atomic_flag_wait_explicit()`, or
`std::atomic_flag::wait()`) on `*object`, then unblocks *at least* one such
thread; otherwise does nothing.

Equivalent to `object->notify_one()`.

### Parameters

- **object** — pointer to the `atomic_flag` object to notify

### Return value

(none)

### Notes

This form of change-detection is often more efficient than simple polling or
pure spinlocks.

### Example

### See also

- **notify_one (C++20)** — notifies at least one thread waiting on the atomic
  object (public member function of `std::atomic_flag`)
- **notify_all (C++20)** — notifies all threads blocked waiting on the atomic
  object (public member function of `std::atomic_flag`)
- **atomic_flag_notify_all (C++20)** — notifies all threads blocked in
  atomic_flag_wait (function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_flag_notify_one*
