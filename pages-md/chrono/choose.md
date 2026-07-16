# std::chrono::choose

```cpp
enum class choose {
    earliest,
    latest
};  // (since C++20)
```

The scoped enumeration `choose` can be passed to certain member functions of
`std::chrono::time_zone` and `std::chrono::zoned_time` to control how ambiguous
or nonexistent local times should be resolved. Passing `choose::earliest` causes
the earlier time point to be returned, while passing `choose::latest` causes the
later time point to be returned. (For nonexistent local times, these two time
points are identical.)

If a `choose` is not passed and an ambiguous or nonexistent local time is
encountered, a `std::chrono::ambiguous_local_time` or
`std::chrono::nonexistent_local_time` exception (as applicable) will be thrown.

### See also

- **(constructor)** — constructs a `zoned_time` (public member function of
  `std::chrono::zoned_time<Duration,TimeZonePtr>`)
- **to_sys** — converts a `local_time` in this time zone to a `sys_time` (public
  member function of `std::chrono::time_zone`)

---
*Source: https://en.cppreference.com/w/cpp/chrono/choose*
