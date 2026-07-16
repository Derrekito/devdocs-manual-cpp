# std::chrono::system_clock::now

```cpp
static std::chrono::time_point<std::chrono::system_clock> now() noexcept;  // (since C++11)
```

Returns a time point representing the current point in time.

### Parameters

(none)

### Return value

A time point representing the current time.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    const auto now = std::chrono::system_clock::now();
    const std::time_t t_c = std::chrono::system_clock::to_time_t(now);
    std::cout << "The system clock is currently at " << std::ctime(&t_c);
}
```

Possible output:

```text
The system clock is currently at Thu Mar 30 13:28:27 2023
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/system_clock/now*
