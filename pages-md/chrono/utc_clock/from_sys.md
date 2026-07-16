# std::chrono::utc_clock::from_sys

```cpp
template< class Duration >
static std::chrono::utc_time<std::common_type_t<Duration, std::chrono::seconds>>
    from_sys( const std::chrono::sys_time<Duration>& t );  // (since C++20)
```

Converts a `sys_time` `t` to a `utc_time` `u` that represents the same point in
time.

`u.time_since_epoch() - t.time_since_epoch()` is equal to the number of leap
seconds that was inserted between `t` and 1 January 1970. If `t` is the exact
date of a leap second insertion, that leap second is counted as inserted.

### Return value

A `utc_time` representing the same point in time as `t`.

### See also

- **to_sys [static]** — converts `utc_time` to `sys_time` (public static member
  function)
- **clock_cast (C++20)** — convert time points of one clock to another (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/utc_clock/from_sys*
