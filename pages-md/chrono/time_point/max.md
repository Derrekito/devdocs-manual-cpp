# std::chrono::time_point<Clock,Duration>::max

```cpp
static constexpr time_point max();  // (until C++20)
static constexpr time_point max() noexcept;  // (since C++20)
```

Returns a `time_point` with the largest possible duration, i.e.
`time_point(std::chrono::duration::max())`.

### Parameters

(none)

### Return value

The largest possible `time_point`.

### Example

```cpp
#include <chrono>
#include <iostream>
#include <vector>

int main()
{
    std::chrono::time_point<std::chrono::system_clock> now =
        std::chrono::system_clock::now();
    std::vector<std::chrono::time_point<std::chrono::system_clock>> times
    {
        now - std::chrono::hours(24),
        now - std::chrono::hours(48),
        now + std::chrono::hours(24)
    };

    std::chrono::time_point<std::chrono::system_clock> earliest =
        std::chrono::time_point<std::chrono::system_clock>::max();

    std::cout << "all times:\n";
    for (const auto& time : times)
    {
        std::time_t t = std::chrono::system_clock::to_time_t(time);
        std::cout << std::ctime(&t);

        if (time < earliest)
            earliest = time;
    }

    std::time_t t = std::chrono::system_clock::to_time_t(earliest);
    std::cout << "earliest:\n" << std::ctime(&t);
}
```

Possible output:

```text
all times:
Sun Oct  7 19:06:48 2012
Sat Oct  6 19:06:48 2012
Tue Oct  9 19:06:48 2012
earliest:
Sat Oct  6 19:06:48 2012
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/time_point/max*
