# std::chrono::time_point<Clock,Duration>::min

```cpp
static constexpr time_point min();  // (until C++20)
static constexpr time_point min() noexcept;  // (since C++20)
```

Returns a `time_point` with the smallest possible duration, i.e.
`time_point(std::chrono::duration::min())`.

### Parameters

(none)

### Return value

The smallest possible `time_point`.

### Example

```cpp
#include <chrono>
#include <iomanip>
#include <iostream>
#include <ratio>
#include <string>

constexpr auto steady_min = std::chrono::steady_clock::time_point::min();

void animate_frame_at_time_offset(double game_time)
{
    std::cout << std::string(static_cast<int>(game_time) % 10 + 1, '*') << '\n';
}

int main()
{
    auto last_frame = steady_min;
    std::chrono::duration<double, std::micro> game_time{0.0};

    for (int n = 0; n < 5; ++n)
    {
        const auto current_frame = std::chrono::steady_clock::now();
        // initialize timer if first frame ever:
        if (last_frame == steady_min)
            last_frame = current_frame;
        game_time += current_frame - last_frame;
        std::cout << "Drawing frame at " << std::setprecision(10)
                  << std::setw(8) << game_time.count() << " μs ";
        animate_frame_at_time_offset(game_time.count());
    }
}
```

Possible output:

```text
Drawing frame at        0 μs *
Drawing frame at  134.499 μs *****
Drawing frame at  274.337 μs *****
Drawing frame at  416.571 μs *******
Drawing frame at  561.124 μs **
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/time_point/min*
