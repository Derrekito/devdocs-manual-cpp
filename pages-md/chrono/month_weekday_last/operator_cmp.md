# std::chrono::operator==(std::chrono::month_weekday_last)

```cpp
constexpr bool operator==( const std::chrono::month_weekday_last& x,
                           const std::chrono::month_weekday_last& y ) noexcept;  // (since C++20)
```

Compares the two `month_weekday_last` values `x` and `y`.

The `!=` operator is synthesized from `operator==`.

### Return value

`x.month() == y.month() && x.weekday_last() == y.weekday_last()`

### Example

```cpp
#include <chrono>

int main()
{
    constexpr std::chrono::month_weekday_last mwdl1
    {
        std::chrono::March/std::chrono::Friday[std::chrono::last]
    };

    constexpr std::chrono::month_weekday_last mwdl2
    {
        std::chrono::March, std::chrono::Friday[std::chrono::last]
    };

    static_assert(mwdl1 == mwdl2);
}
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_weekday_last/operator_cmp*
