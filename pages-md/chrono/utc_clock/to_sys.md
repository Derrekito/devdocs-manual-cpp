# std::chrono::utc_clock::to_sys

```cpp
template< class Duration >
static std::chrono::sys_time<std::common_type_t<Duration, std::chrono::seconds>>
    to_sys( const std::chrono::utc_time<Duration>& t );  // (since C++20)
```

Converts a `utc_time` `t` to a `sys_time` representing the same point in time
(if possible).

If `t` represents a time point during a leap second insertion, the last
representable value of `sys_time` prior to the insertion of the leap second is
returned. In all other cases, `utc_clock::from_sys(utc_clock::to_sys(t)) == t`.

### Return value

A `sys_time` representing the same point in time as `t`, or the last
representable value prior to the insertion of the leap second if `t` represents
a time point during a leap second insertion.

### See also

- **from_sys [static]** — converts `sys_time` to `utc_time` (public static
  member function)
- **clock_cast (C++20)** — convert time points of one clock to another (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/utc_clock/to_sys*
