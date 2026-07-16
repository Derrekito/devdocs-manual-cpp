# std::counting_semaphore<LeastMaxValue>::try_acquire_until

```cpp
template< class Clock, class Duration >
bool try_acquire_until( const std::chrono::time_point<Clock, Duration>& abs_time );  // (since C++20)
```

Tries to atomically decrement the internal counter by `1` if it is greater than
`​0​`; otherwise blocks until it is greater than `​0​` and can successfully
decrement the internal counter, or the `abs_time` time point has been passed.

The programs is ill-formed if `std::chrono::is_clock_v<Clock>` is `false`.

### Preconditions

`Clock` meets the Clock requirements.

### Parameters

- **abs_time** — the *earliest* time the function must wait until in order to
  fail

### Return value

`true` if it decremented the internal counter, otherwise `false`.

### Exceptions

May throw `std::system_error` or a timeout-related exception.

### Notes

In practice the function may take longer than `abs_time` to fail.

### See also

- **release** — increments the internal counter and unblocks acquirers (public
  member function)
- **acquire** — decrements the internal counter or blocks until it can (public
  member function)
- **try_acquire** — tries to decrement the internal counter without blocking
  (public member function)
- **try_acquire_for** — tries to decrement the internal counter, blocking for up
  to a duration time (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/counting_semaphore/try_acquire_until*
