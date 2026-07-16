# std::hash<std::chrono::zoned_time>

```cpp
template< class Duration, class TimeZonePtr >
struct hash<std::chrono::zoned_time<Duration, TimeZonePtr>>;  // (since C++26)
```

The template specialization of `std::hash` for `std::chrono::zoned_time` allows
users to obtain hashes of objects of type std::chrono::zoned_time<Duration,
TimeZonePtr>. This specialization is enabled if and only if both
std::hash<Duration> and std::hash<TimeZonePtr> are enabled.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_chrono` | 202306L | (C++26) | Hashing support for `std::chrono`
      value classes

### See also

- **hash (C++11)** — hash function object (class template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/zoned_time/hash*
