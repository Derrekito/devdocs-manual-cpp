# std::chrono::year::operator int

```cpp
constexpr explicit operator int() const noexcept;  // (since C++20)
```

Returns the year value stored in `*this`.

### Return value

The year value stored in `*this`.

### Example

```cpp
#include <chrono>
#include <iostream>
using namespace std::chrono;

int main()
{
    constexpr std::chrono::year y{2020};
    std::cout << "The year is: " << static_cast<int>(y) << '\n';

    const year_month_day ymd{floor<days>(system_clock::now())};
    const std::chrono::year this_year{ymd.year()};
    std::cout << "This year is: " << int(this_year) << '\n';
}
```

Possible output:

```text
The year is: 2020
This year is: 2023
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/year/operator_int*
