# std::chrono::day::operator++, std::chrono::day::operator--

```cpp
constexpr std::chrono::day& operator++() noexcept;  // (1) (since C++20)
constexpr std::chrono::day operator++( int ) noexcept;  // (2) (since C++20)
constexpr std::chrono::day& operator--() noexcept;  // (3) (since C++20)
constexpr std::chrono::day operator--( int ) noexcept;  // (4) (since C++20)
```

Adds or subtracts 1 from the day value.

1,2) Performs `*this += std::chrono::days{1};`.

3,4) Performs `*this -= std::chrono::days{1};`.

### Parameters

(none)

### Return value

1,3) A reference to this `day` after modification.

2,4) A copy of the `day` made before modification.

### Notes

If the result would be outside the range `[``​0​``,``255``]`, the actual stored
value is unspecified.

### Example

```cpp
#include <cassert>
#include <chrono>

int main()
{
    std::chrono::day d{15};

    ++d;
    assert(d == std::chrono::day(16));

    --d;
    assert(d == std::chrono::day(15));
}
```

### See also

- **operator+=operator-=** — adds or subtracts a number of days (public member
  function)
- **operator+operator- (C++20)** — adds or subtracts a number of days and a
  `day`, or find the difference between two `day`s (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/day/operator_inc_dec*
