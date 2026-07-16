# std::shared_lock<Mutex>::try_lock

```cpp
bool try_lock();  // (since C++14)
```

Tries to lock the associated mutex in shared mode without blocking. Effectively
calls `mutex()->try_lock_shared()`.

`std::system_error` is thrown if there is no associated mutex or if the mutex is
already locked.

### Parameters

(none)

### Return value

`true` if the ownership of the mutex has been acquired successfully, `false`
otherwise.

### Exceptions

- Any exceptions thrown by `mutex()->try_lock_shared()`.
- If there is no associated mutex, `std::system_error` with an error code of
  `std::errc::operation_not_permitted`.
- If the mutex is already locked, `std::system_error` with an error code of
  `std::errc::resource_deadlock_would_occur`.

### Example

### See also

- **lock** — locks the associated mutex (public member function)
- **try_lock** — tries to lock (i.e., takes ownership of) the associated mutex
  without blocking (public member function of `std::unique_lock<Mutex>`)
- **try_lock_for** — tries to lock the associated mutex, for the specified
  duration (public member function)
- **try_lock_until** — tries to lock the associated mutex, until a specified
  time point (public member function)
- **unlock** — unlocks the associated mutex (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_lock/try_lock*
