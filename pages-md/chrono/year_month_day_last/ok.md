# std::chrono::year_month_day_last::ok

```cpp
constexpr bool ok() const noexcept;  // (since C++20)
```

Checks if `*this` represents a valid date. Because a `year_month_day_last`
represents the last day of a particular month, it represents a valid date as
long as the year and month are valid.

### Return value

`year().ok() && month().ok()`

### Example

```cpp
#include <cassert>
#include <chrono>

int main()
{
    auto ymdl{std::chrono::last/11/2020};
    assert(ymdl.ok());
    ymdl = std::chrono::year(2020)/std::chrono::month(13)/std::chrono::last;
    assert(not ymdl.ok());
}
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_day_last/ok*
