# std::chrono::file_clock::to_utc, std::chrono::file_clock::from_utc

```cpp
template< class Duration >
static std::chrono::utc_time</* see below */>
    to_utc( const std::chrono::file_time<Duration>& t );  // (1) (since C++20) (optional)
template< class Duration >
static std::chrono::file_time</* see below */>
    from_utc( const std::chrono::utc_time<Duration>& t );  // (2) (since C++20) (optional)
```

1) Converts the `file_time` `t` to a `utc_time` representing the same point in
   time.

2) Converts the `utc_time` `t` to a `file_time` representing the same point in
   time.

The duration of the return type is computed from `Duration` in an unspecified
manner.

These function templates are optional: an implementation may choose to instead
provide `to_sys` and `from_sys`.

### Return value

1) A `utc_time` representing the same point in time as the argument.

2) A `file_time` representing the same point in time as the argument.

### Notes

User code should usually use `std::chrono::clock_cast`, which provides a generic
interface to convert time points between clocks, rather than call these
functions directly.

### Example

### See also

- **to_sysfrom_sys [static] (optional)** — converts between `file_time` and
  `sys_time` (public static member function)
- **clock_cast (C++20)** — convert time points of one clock to another (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/file_clock/to_from_utc*
