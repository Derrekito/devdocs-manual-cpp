# std::chrono::operator==(std::chrono::year_month_weekday_last)

```cpp
constexpr bool operator==( const std::chrono::year_month_weekday_last& x,
                           const std::chrono::year_month_weekday_last& y ) noexcept;  // (since C++20)
```

Compares the two `year_month_weekday_last` values `x` and `y`.

The `!=` operator is synthesized from `operator==`.

### Return value

`x.year() == y.year() && x.month() == y.month() && x.weekday() == y.weekday()`

### Example

```cpp
#include <chrono>
#include <iostream>
using namespace std::chrono;

int main()
{
    constexpr auto ymwdl1{Tuesday[last]/11/2021};
    auto ymwdl2 = Tuesday[last]/11/2020;
    ymwdl2 += months(12);
    std::cout << std::boolalpha << (ymwdl1 == ymwdl2) << '\n';
}
```

Output:

```text
true
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_weekday_last/operator_cmp*
