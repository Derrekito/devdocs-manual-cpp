# std::counting_semaphore<LeastMaxValue>::try_acquire_for

```cpp
template< class Rep, class Period >
bool try_acquire_for( const std::chrono::duration<Rep, Period>& rel_time );  // (since C++20)
```

Tries to atomically decrement the internal counter by `1` if it is greater than
`​0​`; otherwise blocks until it is greater than `​0​` and can successfully
decrement the internal counter, or the `rel_time` duration has been exceeded.

### Preconditions

(none)

### Parameters

- **rel_time** — the *minimum* duration the function must wait for it to fail

### Return value

`true` if it decremented the internal counter, otherwise `false`.

### Exceptions

May throw `std::system_error` or a timeout-related exception.

### Notes

In practice the function may take longer than `rel_time` to fail.

### See also

- **release** — increments the internal counter and unblocks acquirers (public
  member function)
- **acquire** — decrements the internal counter or blocks until it can (public
  member function)
- **try_acquire** — tries to decrement the internal counter without blocking
  (public member function)
- **try_acquire_until** — tries to decrement the internal counter, blocking
  until a point in time (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/counting_semaphore/try_acquire_for*
