# std::chrono::zoned_time<Duration,TimeZonePtr>::get_info

```cpp
std::chrono::sys_info get_info() const;  // (since C++20)
```

Obtains the `std::chrono::sys_info` containing information about the time zone
at the time point stored in `*this`.

### Return value

`zone->get_info(tp)`, where `zone` is the non-static data member holding the
time zone pointer, and `tp` is the non-static data member holding the stored
time point (as a `std::chrono::sys_time<duration>`).

---
*Source: https://en.cppreference.com/w/cpp/chrono/zoned_time/get_info*
