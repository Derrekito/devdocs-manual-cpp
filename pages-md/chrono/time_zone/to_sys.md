# std::chrono::time_zone::to_sys

```cpp
template< class Duration >
auto to_sys( const std::chrono::local_time<Duration>& tp ) const
    -> std::chrono::sys_time<std::common_type_t<Duration, std::chrono::seconds>>;  // (since C++20)
template< class Duration >
auto to_sys( const std::chrono::local_time<Duration>& tp, std::chrono::choose z ) const
    -> std::chrono::sys_time<std::common_type_t<Duration, std::chrono::seconds>>;  // (since C++20)
```

Converts the `local_time` `tp` in this time zone to the corresponding
`sys_time`.

1) Throws an exception if the conversion is ambiguous or if `tp` represents a
   nonexistent time.
2) Resolves ambiguity according to the value of `z`:

- If `z == std::chrono::choose::earliest`, returns the earlier `sys_time`.
- If `z == std::chrono::choose::latest`, returns the later `sys_time`.

If `tp` represents a nonexistent time between two UTC `time_point`s, those two
   `time_point`s will be the same, and that `time_point` will be returned.

### Return value

The UTC equivalent of `tp` according to the rules of this time zone.

### Exceptions

1) Throws:

- `std::chrono::ambiguous_local_time` if the conversion is ambiguous,
- `std::chrono::nonexistent_local_time` if `tp` represents a nonexistent time.

### Notes

The precision of the result is at least `std::chrono::seconds`, and will be
finer if the argument has finer precision.

Ambiguous and nonexistent local times can occur as a result of time zone
transitions (such as daylight saving time). For example, `"2016-03-13 02:30:00"`
does not exist in the `"America/New_York"` time zone, while `"2016-11-06
01:30:00"` in that time zone can correspond to two UTC time points: `2016-11-06
05:30:00 UTC` and `2016-11-06 06:30:00 UTC`.

---
*Source: https://en.cppreference.com/w/cpp/chrono/time_zone/to_sys*
