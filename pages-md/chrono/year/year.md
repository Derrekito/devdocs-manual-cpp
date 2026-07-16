# std::chrono::year::year

```cpp
year() = default;  // (1) (since C++20)
constexpr explicit year( int y ) noexcept;  // (2) (since C++20)
```

Constructs a `year` object.

1) Default constructor leaves the year value uninitialized.

2) If `y` is in the range `[``-32767``,``32767``]`, constructs a `year` object
   holding the year value `y`. Otherwise the value held is unspecified.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    using namespace std::chrono;

    constexpr int leap_years = []
    {
        int count{};
        for (int i{year::min()}; i <= int{year::max()}; ++i)
            if (year{i}.is_leap()) // uses constructor (2)
                ++count;
        return count;
    } ();

    static_assert(15891 == leap_years);

    std::cout << "There are " << leap_years << " leap years in the range ["
              << int(year::min()) << ", " << int(year::max()) << "].\n";
}
```

Output:

```text
There are 15891 leap years in the range [-32767, 32767].
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/year/year*
