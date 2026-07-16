# std::chrono::get_leap_second_info

```cpp
template< class Duration >
std::chrono::leap_second_info
    get_leap_second_info( const std::chrono::utc_time<Duration>& ut );  // (since C++20)
```

Obtains a `std::chrono::leap_second_info` whose `is_leap_second` and `elapsed`
indicate whether `ut` is during a positive leap second insertion and the sum of
leap seconds between 1 January 1970 and `ut`, respectively.

### Parameters

- **ut** — UTC time to examine

### Return value

A `leap_second_info` value specified above.

### Exceptions

May throw implementation-defined exceptions.

### See also

- **leap_second_info (C++20)** — leap second insertion information (class)
- **leap_second (C++20)** — contains information about a leap second insertion
  (class)

---
*Source: https://en.cppreference.com/w/cpp/chrono/utc_clock/get_leap_second_info*
