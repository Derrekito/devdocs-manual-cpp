# std::chrono::system_clock::to_time_t

```cpp
static std::time_t to_time_t( const time_point& t ) noexcept;  // (since C++11)
```

Converts `t` to a `std::time_t` type.

If `std::time_t` has lower precision, it is implementation-defined whether the
value is rounded or truncated.

### Parameters

- **t** — system clock time point to convert

### Return value

A `std::time_t` value representing `t`.

### Example

Get the current time as a `std::time_t` two ways.

```cpp
#include <chrono>
#include <ctime>
#include <iostream>
#include <thread>
using namespace std::chrono_literals;

int main()
{
    // The old way
    std::time_t oldt = std::time({});

    std::this_thread::sleep_for(2700ms);

    // The new way
    auto const now = std::chrono::system_clock::now();
    std::time_t newt = std::chrono::system_clock::to_time_t(now);

    std::cout << "newt - oldt == " << newt - oldt << " s\n";
}
```

Possible output:

```text
newt - oldt == 3 s
```

### See also

- **from_time_t [static]** — converts `std::time_t` to a system clock time point
  (public static member function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/system_clock/to_time_t*
