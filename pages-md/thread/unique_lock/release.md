# std::unique_lock<Mutex>::release

```cpp
mutex_type* release() noexcept;  // (since C++11)
```

Breaks the association of the associated mutex, if any, and `*this`.

No locks are unlocked. If `*this` held ownership of the associated mutex prior
to the call, the caller is now responsible to unlock the mutex.

### Parameters

(none)

### Return value

Pointer to the associated mutex or a null pointer if there was no associated
mutex.

### Example

### See also

- **unlock** — unlocks (i.e., releases ownership of) the associated mutex
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/unique_lock/release*
