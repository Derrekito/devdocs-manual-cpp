# std::shared_timed_mutex::try_lock_shared_for

```cpp
template< class Rep, class Period >
bool try_lock_shared_for( const std::chrono::duration<Rep,Period>& timeout_duration );  // (since C++14)
```

Tries to lock the mutex in shared mode. Blocks until specified
`timeout_duration` has elapsed or the shared lock is acquired, whichever comes
first. On successful lock acquisition returns `true`, otherwise returns `false`.

If `timeout_duration` is less or equal `timeout_duration.zero()`, the function
behaves like `try_lock_shared()`.

This function may block for longer than `timeout_duration` due to scheduling or
resource contention delays.

The standard recommends that a steady clock is used to measure the duration. If
an implementation uses a system clock instead, the wait time may also be
sensitive to clock adjustments.

As with `try_lock_shared()`, this function is allowed to fail spuriously and
return `false` even if the mutex was not locked by any other thread at some
point during `timeout_duration`.

Prior `unlock()` operation on the same mutex *synchronizes-with* (as defined in
`std::memory_order`) this operation if it returns `true`.

If `try_lock_shared_for` is called by a thread that already owns the `mutex` in
any mode (shared or exclusive), the behavior is undefined.

### Parameters

- **timeout_duration** — maximum duration to block for

### Return value

`true` if the lock was acquired successfully, otherwise `false`.

### Exceptions

Any exception thrown by clock, time_point, or duration during the execution
(clocks, time points, and durations provided by the standard library never
throw).

### Example

### See also

- **try_lock_shared** — tries to lock the mutex for shared ownership, returns if
  the mutex is not available (public member function)
- **try_lock_shared_until** — tries to lock the mutex for shared ownership,
  returns if the mutex has been unavailable until specified time point has been
  reached (public member function)
- **try_lock_for** — tries to lock the mutex, returns if the mutex has been
  unavailable for the specified timeout duration (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_timed_mutex/try_lock_shared_for*
