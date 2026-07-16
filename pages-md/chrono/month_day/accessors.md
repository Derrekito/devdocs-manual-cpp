# std::chrono::month_day::month, std::chrono::month_day::day

```cpp
constexpr std::chrono::month month() const noexcept;  // (1) (since C++20)
constexpr std::chrono::day day() const noexcept;  // (1) (since C++20)
```

Retrieves the month and day values stored in this `month_day` object.

### Return value

1) Returns the stored `std::chrono::month` value.

2) Returns the stored `std::chrono::day` value.

### Example

```cpp
#include <chrono>

int main()
{
    constexpr auto md{std::chrono::July/15};
    static_assert(md.month() == std::chrono::month(7));
    static_assert(md.day() == std::chrono::day(15));
}
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_day/accessors*
