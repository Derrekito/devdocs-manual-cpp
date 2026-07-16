# std::chrono::weekday::operator[]

```cpp
constexpr std::chrono::weekday_indexed
    operator[]( unsigned index ) const noexcept;  // (1) (since C++20)
constexpr std::chrono::weekday_last
    operator[]( std::chrono::last_spec ) const noexcept;  // (2) (since C++20)
```

1) Constructs a `weekday_indexed` from `*this` and `index`. The result
   represents the `index`-th weekday in some yet-to-be-specified month. If
   `index` is not in the range `[``​0​``,``7``]` or if `!ok()` the values (an
   underlying weekday and an index) held in the result are unspecified.

2) Constructs a `weekday_last` from `*this`. The result represents the last
   weekday in some yet-to-be-specified month.

### Return value

1) `std::chrono::weekday_indexed(*this, index)`

2) `std::chrono::weekday_last(*this)`

### Example

```cpp
#include <chrono>
#include <iostream>
using namespace std::chrono;

int main()
{
    constexpr auto second_tuesday_in_October_2019 =
        year_month_day{Tuesday[2] / October / 2019y};

    constexpr auto last_tuesday_in_October_2019 =
        year_month_day{Tuesday[last] / October / 2019y};

    std::cout << second_tuesday_in_October_2019 << '\n'
              << last_tuesday_in_October_2019 << '\n';
}
```

Possible output:

```text
2019-10-08
2019-10-29
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/weekday/operator_at*
