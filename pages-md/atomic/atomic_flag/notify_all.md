# std::atomic_flag::notify_all

```cpp
void notify_all() noexcept;  // (1) (since C++20)
void notify_all() volatile noexcept;  // (2) (since C++20)
```

Performs atomic notifying operations.

Unblocks all threads blocked in atomic waiting operations (i.e. `wait()`) on
`*this`, if there are any; otherwise does nothing.

### Return value

(none)

### Notes

This form of change-detection is often more efficient than simple polling or
pure spinlocks.

### Example

### See also

- **wait (C++20)** — blocks the thread until notified and the atomic value
  changes (public member function)
- **atomic_flag_waitatomic_flag_wait_explicit (C++20)(C++20)** — blocks the
  thread until notified and the flag changes (function)
- **atomic_flag_notify_one (C++20)** — notifies a thread blocked in
  atomic_flag_wait (function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_flag/notify_all*
