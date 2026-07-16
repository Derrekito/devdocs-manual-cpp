# std::chrono::operator==,<=>(std::chrono::year)

```cpp
constexpr bool operator==( const std::chrono::year& x,
                           const std::chrono::year& y ) noexcept;  // (1) (since C++20)
constexpr std::strong_ordering operator<=>( const std::chrono::year& x,
                                            const std::chrono::year& y ) noexcept;  // (2) (since C++20)
```

Compare the two `std::chrono::year` `x` and `y`.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.

### Return value

1) `int(x) == int(y)`

2) `int(x) <=> int(y)`

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    using namespace std::chrono;

    constexpr year y1{2020};
    constexpr year y2{2021};

    std::cout << std::boolalpha << (y1 != y2) << '\n';

    static_assert((2020y < 2023y) && (2020y == 2020y) && (2020y <= 2023y) &&
                  (2023y > 2020y) && (2023y != 2020y) && (2023y >= 2020y));
}
```

Output:

```text
true
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/year/operator_cmp*
