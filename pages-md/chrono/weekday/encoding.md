# std::chrono::weekday::c_encoding, std::chrono::weekday::iso_encoding

```cpp
constexpr unsigned c_encoding() const noexcept;  // (1) (since C++20)
constexpr unsigned iso_encoding() const noexcept;  // (2) (since C++20)
```

1) Returns the weekday value stored in `*this`.

2) Returns the weekday value stored in `*this`, except that Sunday (stored as
   `​0​`) is interpreted as `7` according to ISO 8601.

### Return value

1) The weekday value stored in `*this`.

2) `c_encoding() == 0u ? 7u : c_encoding()`

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    std::cout << "i: C: ISO: Weekday:\n";

    for (unsigned i{0}; i != 8; ++i)
    {
        const std::chrono::weekday w{i};
        std::cout << i << "  "
                  << w.c_encoding() << "  "
                  << w.iso_encoding() << "    "
                  << w << '\n';
    }
}
```

Output:

```text
i: C: ISO: Weekday:
0  0  7    Sun
1  1  1    Mon
2  2  2    Tue
3  3  3    Wed
4  4  4    Thu
5  5  5    Fri
6  6  6    Sat
7  0  7    Sun
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/weekday/encoding*
