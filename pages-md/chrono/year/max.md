# std::chrono::year::max

```cpp
static constexpr year max() noexcept;  // (since C++20)
```

Returns the largest possible `year`, that is, `std::chrono::year(32767)`.

### Return value

`std::chrono::year(32767)`

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    std::cout << "The maximum year is: " << (int)std::chrono::year::max() << '\n';
}
```

Output:

```text
The maximum year is: 32767
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/year/max*
