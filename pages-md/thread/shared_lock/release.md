# std::shared_lock<Mutex>::release

```cpp
mutex_type* release() noexcept;  // (since C++14)
```

Breaks the association of the associated mutex, if any, and `*this`.

No locks are unlocked. If the `*this` held ownership of the associated mutex
prior to the call, the caller is now responsible to unlock the mutex.

### Parameters

(none)

### Return value

Pointer to the associated mutex or a null pointer if there was no associated
mutex.

### Example

### See also

- **unlock** — unlocks the associated mutex (public member function)
- **release** — disassociates the associated mutex without unlocking (i.e.,
  releasing ownership of) it (public member function of
  `std::unique_lock<Mutex>`)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_lock/release*
