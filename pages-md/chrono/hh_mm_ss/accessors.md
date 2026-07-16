# std::chrono::hh_mm_ss<Duration>::is_negative, std::chrono::hh_mm_ss<Duration>::hours, std::chrono::hh_mm_ss<Duration>::minutes, std::chrono::hh_mm_ss<Duration>::seconds, std::chrono::hh_mm_ss<Duration>::subseconds

```cpp
constexpr bool is_negative() const noexcept;  // (1)
constexpr std::chrono::hours hours() const noexcept;  // (2)
constexpr std::chrono::minutes minutes() const noexcept;  // (3)
constexpr std::chrono::seconds seconds() const noexcept;  // (4)
constexpr precision subseconds() const noexcept;  // (5)
```

Obtains the components of the stored "broken down" time.

### Return value

Let `d` be the represented duration:

1) `true` if `d` is negative, `false` otherwise.

2) `std::chrono::duration_cast<std::chrono::hours>(abs(d))`

3) `std::chrono::duration_cast<std::chrono::minutes>(abs(d) - hours())`

4) `std::chrono::duration_cast<std::chrono::seconds>(abs(d) - hours() -
   minutes())`

5) `abs(d) - hours() - minutes() - seconds()` if
   `std::chrono::treat_as_floating_point_v<precision::rep>` is `true`; otherwise
   `std::chrono::duration_cast<precision>(abs(d) - hours() - minutes() -
   seconds())`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/chrono/hh_mm_ss/accessors*
