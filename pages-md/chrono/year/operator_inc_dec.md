# std::chrono::year::operator++, std::chrono::year::operator--

```cpp
constexpr std::chrono::year& operator++() noexcept;  // (1) (since C++20)
constexpr std::chrono::year operator++( int ) noexcept;  // (2) (since C++20)
constexpr std::chrono::year& operator--() noexcept;  // (3) (since C++20)
constexpr std::chrono::year operator--( int ) noexcept;  // (4) (since C++20)
```

Adds or subtracts 1 from the year value.

1,2) Performs `*this += std::chrono::years{1};`.

3,4) Performs `*this -= std::chrono::years{1};`.

### Parameters

(none)

### Return value

1,3) A reference to this `year` after modification.

2,4) A copy of the `year` made before modification.

### Notes

If the result would be outside the range `[``-32767``,``32767``]`, the actual
stored value is unspecified.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    std::cout << std::boolalpha;

    std::chrono::year y{2020};
    std::cout << (++y == std::chrono::year(2021)) << ' ';
    std::cout << (--y == std::chrono::year(2020)) << '\n';

    using namespace std::literals::chrono_literals;
    y = 32767y;
    y++; //← unspecified, see ↑ Notes ↑
    std::cout << static_cast<int>(y) << '\n';
}
```

Possible output:

```text
true true
-32768
```

### See also

- **operator+=operator-=** — adds or subtracts a number of years from a `year`
  (public member function)
- **operator+operator- (C++20)** — performs arithmetic on `year`s (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year/operator_inc_dec*
