# std::shared_timed_mutex::unlock_shared

```cpp
void unlock_shared();  // (since C++14)
```

Releases the mutex from shared ownership by the calling thread.

The mutex must be locked by the current thread of execution in shared mode,
otherwise, the behavior is undefined.

This operation *synchronizes-with* (as defined in `std::memory_order`) any
subsequent `lock()` operation that obtains ownership of the same mutex.

### Parameters

(none)

### Return value

(none)

### Exceptions

Throws nothing.

### Notes

`unlock_shared()` is usually not called directly: `std::shared_lock` is used to
manage shared locking.

### Example

### See also

- **lock_shared** — locks the mutex for shared ownership, blocks if the mutex is
  not available (public member function)
- **unlock** — unlocks the mutex (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_timed_mutex/unlock_shared*
