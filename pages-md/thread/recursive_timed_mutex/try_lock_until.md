# std::recursive_timed_mutex::try_lock_until

```cpp
template< class Clock, class Duration >
bool try_lock_until( const std::chrono::time_point<Clock, Duration>& timeout_time );  // (since C++11)
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

A thread may call `try_lock_until` on a recursive mutex repeatedly. Successful
calls to `try_lock_until` increment the ownership count: the mutex will only be
released after the thread makes a matching number of calls to `unlock`.

The maximum number of levels of ownership is unspecified. A call to
`try_lock_until` will return `false` if this number is exceeded.

### Parameters

- **timeout_time** — maximum time point to block until

### Return value

`true` if the lock was acquired successfully, otherwise `false`.

### Exceptions

Any exception thrown by `timeout_time` (clocks, time points, and durations
provided by the standard library never throw).

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2093 | C++11 | `try_lock_until` threw nothing | throws timeout-related
      exceptions

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
*Source: https://en.cppreference.com/w/cpp/thread/recursive_timed_mutex/try_lock_until*
