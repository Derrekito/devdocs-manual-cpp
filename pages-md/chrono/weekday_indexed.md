# std::chrono::weekday_indexed

```cpp
class weekday_indexed;  // (since C++20)
```

The class `weekday_indexed` combines a `weekday`, representing a day of the week
in the proleptic Gregorian calendar, with a small index `n` in the range
`[`1`,`5`]`. It represents the first, second, third, fourth, or fifth weekday of
some month.

`weekday_indexed` is a TriviallyCopyable StandardLayoutType.

### Member functions

- **(constructor)** — constructs a `weekday_indexed` (public member function)
- **weekday** — access the stored `weekday` (public member function)
- **index** — access the stored index (public member function)
- **ok** — checks if the weekday and index are both valid (public member
  function)

### Nonmember functions

- **operator== (C++20)** — compares two `weekday_indexed` values (function)
- **operator<< (C++20)** — outputs a `weekday_indexed` into a stream (function
  template)

### Helper classes

- **std::formatter<std::chrono::weekday_indexed> (C++20)** — formatting support
  for `weekday_indexed` (class template specialization)
- **std::hash<std::chrono::weekday_indexed> (C++26)** — hash support for
  **`std::chrono::weekday_indexed`** (class template specialization)

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    using namespace std::chrono;

    constexpr weekday_indexed wi = Friday[2];

    // Indexed weekday can be used at any place where chrono::day can be used:
    constexpr year_month_weekday ymwd = 2021y / August / wi;
    static_assert(ymwd == August / wi / 2021y &&
                  ymwd == wi / August / 2021y);
    std::cout << ymwd << '\n';

    constexpr year_month_day ymd{ymwd}; // a conversion
    static_assert(ymd == 2021y / 8 / 13);
    std::cout << ymd << '\n';
}
```

Possible output:

```text
2021/Aug/Fri[2]
2021-08-13
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/weekday_indexed*
