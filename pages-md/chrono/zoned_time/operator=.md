# std::chrono::zoned_time<Duration,TimeZonePtr>::operator=

```cpp
zoned_time& operator=( const zoned_time& other ) = default;  // (1) (since C++20)
zoned_time& operator=( const std::chrono::sys_time<Duration>& other );  // (2) (since C++20)
zoned_time& operator=( const std::chrono::local_time<Duration>& other );  // (3) (since C++20)
```

Assign the value of `other` to `*this`.

1) Defaulted copy assignment operator. Copy-assigns both the stored time point
   and the stored time zone pointer. `zoned_time` has no move assignment
   operator; a move is a copy.

2) Assigns `other` to the stored time point. The time zone pointer is unchanged.
   After this call, `get_sys_time() == other`.

3) Converts `other` to a `std::chrono::sys_time` as if by `zone->to_sys(other)`
   (where `zone` is the non-static data member holding the stored time zone
   pointer) and assigns the result to the stored time point. The time zone
   pointer is unchanged. After this call, `get_local_time() == other`.

### Return value

`*this`

---
*Source: https://en.cppreference.com/w/cpp/chrono/zoned_time/operator%3D*
