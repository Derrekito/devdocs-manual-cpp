# std::atomic_flag_notify_all

```cpp
void atomic_flag_notify_all( std::atomic_flag* object ) noexcept;  // (1) (since C++20)
void atomic_flag_notify_all( volatile std::atomic_flag* object ) noexcept;  // (2) (since C++20)
```

Performs atomic notifying operations.

Unblocks all threads blocked in atomic waiting operations (i.e.
`std::atomic_flag_wait()`, `std::atomic_flag_wait_explicit()`, or
`std::atomic_flag::wait()`) on `*object`, if there are any; otherwise does
nothing.

Equivalent to `object->notify_all()`.

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
- **atomic_flag_waitatomic_flag_wait_explicit (C++20)(C++20)** — blocks the
  thread until notified and the flag changes (function)
- **atomic_flag_notify_one (C++20)** — notifies a thread blocked in
  atomic_flag_wait (function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_flag_notify_all*
