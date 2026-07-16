# std::chrono::operator==,<=>(std::chrono::month_day_last)

```cpp
constexpr bool operator==( const std::chrono::month_day_last& x,
                           const std::chrono::month_day_last& y ) noexcept;  // (1) (since C++20)
constexpr std::strong_ordering operator<=>( const std::chrono::month_day_last& x,
                                            const std::chrono::month_day_last& y ) noexcept;  // (2) (since C++20)
```

Compares the two `month_day_last` values `x` and `y`.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.

### Return value

1) `x.month() == y.month()`

2) `x.month() <=> y.month()`

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    constexpr auto mdl1{std::chrono::February/std::chrono::last};
    constexpr std::chrono::month_day_last mdl2{std::chrono::month(2)};
    std::cout << std::boolalpha << (mdl1 == mdl2) << '\n';

    static_assert(mdl1 <= mdl2);
}
```

Output:

```text
true
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_day_last/operator_cmp*
