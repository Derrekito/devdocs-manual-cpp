# std::chrono::zoned_time<Duration,TimeZonePtr>::operator sys_time<duration>, std::chrono::zoned_time<Duration,TimeZonePtr>::get_sys_time

```cpp
operator std::chrono::sys_time<duration>() const;  // (since C++20)
std::chrono::sys_time<duration> get_sys_time() const;  // (since C++20)
```

Obtains a `std::chrono::sys_time<duration>` representing the same point in time
as this `zoned_time` object.

### Return value

A `std::chrono::sys_time<duration>` representing the same point in time as
`*this`.

---
*Source: https://en.cppreference.com/w/cpp/chrono/zoned_time/get_sys_time*
