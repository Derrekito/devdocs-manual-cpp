# std::chrono::day::day

```cpp
day() = default;  // (1) (since C++20)
constexpr explicit day( unsigned d ) noexcept;  // (2) (since C++20)
```

Constructs a `day` object.

1) Default constructor leaves the day value uninitialized.

2) If `d <= 255`, constructs a `day` object holding the day value `d`. Otherwise
   the value held is unspecified.

---
*Source: https://en.cppreference.com/w/cpp/chrono/day/day*
