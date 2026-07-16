# std::chrono::day::operator unsigned

```cpp
constexpr explicit operator unsigned() const noexcept;  // (since C++20)
```

Returns the day value stored in `*this`.

### Return value

The day value stored in `*this`.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    constexpr std::chrono::day d{15};
    constexpr unsigned day = static_cast<unsigned>(d);
    std::cout << "The day is: " << day << '\n';
}
```

Output:

```text
The day is: 15
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/day/operator_unsigned*
