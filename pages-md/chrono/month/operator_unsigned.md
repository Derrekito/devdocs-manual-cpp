# std::chrono::month::operator unsigned

```cpp
constexpr explicit operator unsigned() const noexcept;  // (since C++20)
```

Returns the month value stored in `*this`.

### Return value

The month value stored in `*this`.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    constexpr std::chrono::month m{6};
    constexpr unsigned p = static_cast<unsigned>(m);
    constexpr unsigned q = static_cast<unsigned>(std::chrono::September);
    std::cout << "The month is: " << p << '\n'
              << "September is: " << q << '\n';
}
```

Output:

```text
The month is: 6
September is: 9
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/month/operator_unsigned*
