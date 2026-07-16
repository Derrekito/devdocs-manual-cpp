# std::chrono::month_day::month_day

```cpp
month_day() = default;  // (1) (since C++20)
constexpr month_day( const std::chrono::month& m,
                     const std::chrono::day& d ) noexcept;  // (2) (since C++20)
```

Constructs a `month_day`.

1) Default constructor leaves the stored month and day values uninitialized.

2) Constructs a `month_day` that stores the month `m` and the day `d`.

### Notes

A more convenient way to construct a `month_day` is with `operator/`, e.g.,
`std::chrono::April/1`.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    using namespace std::chrono_literals;
    constexpr auto mo_da = std::chrono::month_day(std::chrono::February, 29d);
    std::cout << static_cast<unsigned>(mo_da.day()) << '/'
              << static_cast<unsigned>(mo_da.month()) << '\n';
}
```

Output:

```text
29/2
```

### See also

- **operator/ (C++20)** — conventional syntax for Gregorian calendar date
  creation (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_day/month_day*
