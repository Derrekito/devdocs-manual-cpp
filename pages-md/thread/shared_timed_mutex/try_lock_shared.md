# std::shared_timed_mutex::try_lock_shared

```cpp
bool try_lock_shared();  // (since C++14)
```

Tries to lock the mutex in shared mode. Returns immediately. On successful lock
acquisition returns `true`, otherwise returns `false`.

This function is allowed to fail spuriously and return `false` even if the mutex
is not currenly exclusively locked by any other thread.

Prior `unlock()` operation on the same mutex *synchronizes-with* (as defined in
`std::memory_order`) this operation if it returns `true`.

The behavior is undefined if the calling thread already owns the mutex in any
mode.

### Parameters

(none)

### Return value

`true` if the lock was acquired successfully, otherwise `false`.

### Exceptions

Throws nothing.

### Example

### See also

- **lock** — locks the mutex, blocks if the mutex is not available (public
  member function)
- **try_lock** — tries to lock the mutex, returns if the mutex is not available
  (public member function)
- **unlock_shared** — unlocks the mutex (shared ownership) (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_timed_mutex/try_lock_shared*
