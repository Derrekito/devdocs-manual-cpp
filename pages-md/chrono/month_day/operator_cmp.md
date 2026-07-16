# std::chrono::operator==,<=>(std::chrono::month_day)

```cpp
constexpr bool operator==( const std::chrono::month_day& x,
                           const std::chrono::month_day& y ) noexcept;  // (1) (since C++20)
constexpr std::strong_ordering operator<=>( const std::chrono::month_day& x,
                                            const std::chrono::month_day& y ) noexcept;  // (2) (since C++20)
```

Compares two `month_day` values.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.

### Return value

1) `x.month() == y.month() && x.day() == y.day()`

2) `x.month() <=> y.month() != 0 ? x.month() <=> y.month() : x.day() <=>
   y.day()`

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    constexpr auto md1{std::chrono::August/15};
    constexpr auto md2{std::chrono::month(8)/std::chrono::day(15)};
    std::cout << std::boolalpha << (md1 == md2) << '\n';

    static_assert(md1 <= md2);
}
```

Output:

```text
true
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_day/operator_cmp*
