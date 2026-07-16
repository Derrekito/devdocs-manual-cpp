# std::shared_future<T>::wait_until

```cpp
template< class Clock, class Duration >
std::future_status wait_until( const std::chrono::time_point<Clock,Duration>& timeout_time ) const;  // (since C++11)
```

`wait_until` waits for a result to become available. It blocks until specified
`timeout_time` has been reached or the result becomes available, whichever comes
first. The return value indicates why `wait_until` returned.

If the future is the result of a call to `async` that used lazy evaluation, this
function returns immediately without waiting.

The behavior is undefined if `valid()` is `false` before the call to this
function, or `Clock` does not meet the Clock requirements. The programs is
ill-formed if `std::chrono::is_clock_v<Clock>` is `false`.(since C++20)

### Parameters

- **timeout_time** — maximum time point to block until

### Return value

- **`future_status::deferred`** — The shared state contains a deferred function
  using lazy evaluation, so the result will be computed only when explicitly
  requested
- **`future_status::ready`** — The result is ready
- **`future_status::timeout`** — The timeout has expired

### Exceptions

Any exception thrown by clock, time_point, or duration during the execution
(clocks, time points, and durations provided by the standard library never
throw).

### Notes

The implementations are encouraged to detect the case when `valid() == false`
before the call and throw a `std::future_error` with an error condition of
`future_errc::no_state`.

The standard recommends that the clock tied to `timeout_time` be used to measure
time; that clock is not required to be a monotonic clock. There are no
guarantees regarding the behavior of this function if the clock is adjusted
discontinuously, but the existing implementations convert `timeout_time` from
`Clock` to `std::chrono::system_clock` and delegate to POSIX
`pthread_cond_timedwait` so that the wait honors adjustments to the system
clock, but not to the user-provided `Clock`. In any case, the function also may
wait for longer than until after `timeout_time` has been reached due to
scheduling or resource contention delays.

### Example

### See also

- **wait** — waits for the result to become available (public member function)
- **wait_for** — waits for the result, returns if it is not available for the
  specified timeout duration (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_future/wait_until*
