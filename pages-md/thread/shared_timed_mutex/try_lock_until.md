# std::shared_timed_mutex::try_lock_until

```cpp
template< class Clock, class Duration >
bool try_lock_until( const std::chrono::time_point<Clock, Duration>& timeout_time );  // (since C++14)
```

Tries to lock the mutex. Blocks until specified `timeout_time` has been reached
(timeout) or the lock is acquired (owns the mutex), whichever comes first. On
successful lock acquisition returns `true`, otherwise returns `false`.

If `timeout_time` has already passed, this function behaves like `try_lock()`.

`Clock` must meet the Clock requirements. The program is ill-formed if
`std::chrono::is_clock_v<Clock>` is `false`.(since C++20)

The standard recommends that the clock tied to `timeout_time` be used, in which
case adjustments of the clock may be taken into account. Thus, the duration of
the block might be more or less than `timeout_time - Clock::now()` at the time
of the call, depending on the direction of the adjustment and whether it is
honored by the implementation. The function also may block until after
`timeout_time` has been reached due to process scheduling or resource contention
delays.

As with `try_lock()`, this function is allowed to fail spuriously and return
`false` even if the mutex was not locked by any other thread at some point
before `timeout_time`.

Prior `unlock()` operation on the same mutex *synchronizes-with* (as defined in
`std::memory_order`) this operation if it returns `true`.

If `try_lock_until` is called by a thread that already owns the mutex in any
mode (shared or exclusive), the behavior is undefined.

### Parameters

- **timeout_time** — maximum time point to block until

### Return value

`true` if the lock was acquired successfully, otherwise `false`.

### Exceptions

Any exception thrown by `timeout_time` (clocks, time points, and durations
provided by the standard library never throw).

### Example

### See also

- **lock** — locks the mutex, blocks if the mutex is not available (public
  member function)
- **try_lock** — tries to lock the mutex, returns if the mutex is not available
  (public member function)
- **try_lock_for** — tries to lock the mutex, returns if the mutex has been
  unavailable for the specified timeout duration (public member function)
- **unlock** — unlocks the mutex (public member function)

**C documentation for `mtx_timedlock`**

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_timed_mutex/try_lock_until*
