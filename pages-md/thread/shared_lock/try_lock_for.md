# std::shared_lock<Mutex>::try_lock_for

```cpp
template< class Rep, class Period >
bool try_lock_for( const std::chrono::duration<Rep,Period>& timeout_duration );  // (since C++14)
```

Tries to lock the associated mutex in shared mode. Blocks until specified
`timeout_duration` has elapsed or the lock is acquired, whichever comes first.
On successful lock acquisition returns `true`, otherwise returns `false`.
Effectively calls `mutex()->try_lock_shared_for(timeout_duration)`.

This function may block for longer than `timeout_duration` due to scheduling or
resource contention delays.

The standard recommends that a steady clock is used to measure the duration. If
an implementation uses a system clock instead, the wait time may also be
sensitive to clock adjustments.

`std::system_error` is thrown if there is no associated mutex or if the mutex is
already locked.

The behavior is undefined if `Mutex` does not meet the SharedTimedLockable
requirements.

### Parameters

- **timeout_duration** — maximum duration to block for

### Return value

`true` if the ownership of the mutex has been acquired successfully, `false`
otherwise.

### Exceptions

- Any exceptions thrown by `mutex()->try_lock_shared_for(timeout_duration)`.
- If there is no associated mutex, `std::system_error` with an error code of
  `std::errc::operation_not_permitted`.
- If the mutex is already locked, `std::system_error` with an error code of
  `std::errc::resource_deadlock_would_occur`.

### Example

### See also

- **try_lock_for** — attempts to lock (i.e., takes ownership of) the associated
  TimedLockable mutex, returns if the mutex has been unavailable for the
  specified time duration (public member function of `std::unique_lock<Mutex>`)
- **lock** — locks the associated mutex (public member function)
- **try_lock** — tries to lock the associated mutex (public member function)
- **try_lock_until** — tries to lock the associated mutex, until a specified
  time point (public member function)
- **unlock** — unlocks the associated mutex (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_lock/try_lock_for*
