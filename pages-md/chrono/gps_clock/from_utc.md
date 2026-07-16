# std::chrono::gps_clock::from_utc

```cpp
template< class Duration >
static std::chrono::gps_time<std::common_type_t<Duration, std::chrono::seconds>>
    from_utc( const std::chrono::utc_time<Duration>& ) noexcept;  // (since C++20)
```

Converts the `utc_time` `t` to a `gps_time` representing the same point in time.

### Return value

A `std::chrono::gps_time` representing the same point in time as `t`, computed
as if by constructing a value of the return type from `t.time_since_epoch()` and
subtracting `315964809s` (315964809 is the number of seconds between the epochs
of the two clocks: 1980-01-06 00:00:00 UTC for `gps_clock` and 1970-01-01
00:00:00 UTC for `utc_clock`).

### Example

### See also

- **to_utc [static]** — converts a `gps_time` to `utc_time` (public static
  member function)
- **clock_cast (C++20)** — convert time points of one clock to another (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/gps_clock/from_utc*
