# std::chrono::weekday_indexed::weekday_indexed

```cpp
weekday_indexed() = default;  // (1) (since C++20)
constexpr weekday_indexed( const std::chrono::weekday& wd, unsigned index ) noexcept;  // (2) (since C++20)
```

Constructs a `weekday_indexed`.

1) Default constructor leaves both the `std::chrono::weekday` and the index
   value uninitialized.

2) Constructs a `weekday_indexed` storing the weekday `wd` and the index
   `index`. The values held are unspecified if `!wd.ok() || index > 7`.

### Notes

A more convenient way to construct a `weekday_indexed` is with `weekday`'s
`operator[]`, i.e., `wd[index]`.

### Example

```cpp
#include <chrono>
#include <iostream>
using namespace std::chrono;

int main()
{
    constexpr auto third_friday = weekday_indexed(Friday, 3); // uses constructor (2)
    static_assert(third_friday == Friday[3]);

    weekday_indexed wdi = Tuesday[2]; // represents the 2nd Tuesday
    std::cout << year_month_day{ wdi / October / 2019y } << '\n';
}
```

Possible output:

```text
2019-10-08
```

### See also

- **operator[]** — convenience syntax for constructing a `weekday_indexed` or
  `weekday_last` from this `weekday` (public member function of
  `std::chrono::weekday`)

---
*Source: https://en.cppreference.com/w/cpp/chrono/weekday_indexed/weekday_indexed*
