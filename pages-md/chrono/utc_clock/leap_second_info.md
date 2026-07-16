# std::chrono::leap_second_info

```cpp
struct leap_second_info {
    bool is_leap_second;
    std::chrono::seconds elapsed;
};  // (since C++20)
```

A `leap_second_info` indicates whether a UTC time is during a positive leap
second insertion and the total number of leap seconds between 1 January 1970 and
the UTC time.

`leap_second_info` has no base classes or members other than `is_leap_second`,
`elapsed`, and implicitly declared special member functions.

### Member objects

- **is_leap_second** — whether the UTC time is during a positive leap second
  insertion (public member object)
- **elapsed** — the sum of leap seconds between 1 January 1970 and the UTC time
  (public member object)

### Notes

`leap_second_info` is typically obtained from
`std::chrono::get_leap_second_info`.

### See also

- **leap_second (C++20)** — contains information about a leap second insertion
  (class)

---
*Source: https://en.cppreference.com/w/cpp/chrono/utc_clock/leap_second_info*
