# std::unique_lock<Mutex>::try_lock_until

```cpp
template< class Clock, class Duration >
bool try_lock_until( const std::chrono::time_point<Clock, Duration>& timeout_time );  // (since C++11)
```

Tries to lock (i.e., takes ownership of) the associated mutex. Blocks until
specified `timeout_time` has been reached or the lock is acquired, whichever
comes first. On successful lock acquisition returns `true`, otherwise returns
`false`. May block for longer than `timeout_time` until has been reached.

Effectively calls `mutex()->try_lock_until(timeout_time)`.

`std::system_error` is thrown if there is no associated mutex or if the mutex is
already locked by the same thread.

`Clock` must meet the Clock requirements. The program is ill-formed if
`std::chrono::is_clock_v<Clock>` is `false`.(since C++20)

### Parameters

- **timeout_time** — maximum time point to block until

### Return value

`true` if the ownership of the mutex has been acquired successfully, `false`
otherwise.

### Exceptions

- Any exceptions thrown by `mutex()->try_lock_until(timeout_time)`.
- If there is no associated mutex, `std::system_error` with an error code of
  `std::errc::operation_not_permitted`.
- If the mutex is already locked, `std::system_error` with an error code of
  `std::errc::resource_deadlock_would_occur`.

### Example

### See also

- **lock** — locks (i.e., takes ownership of) the associated mutex (public
  member function)
- **try_lock** — tries to lock (i.e., takes ownership of) the associated mutex
  without blocking (public member function)
- **try_lock_for** — attempts to lock (i.e., takes ownership of) the associated
  TimedLockable mutex, returns if the mutex has been unavailable for the
  specified time duration (public member function)
- **unlock** — unlocks (i.e., releases ownership of) the associated mutex
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/unique_lock/try_lock_until*
