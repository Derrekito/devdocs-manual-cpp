# std::hash<std::chrono::time_point>

```cpp
template< class Clock, class Duration >
struct hash<std::chrono::time_point<Clock, Duration>>;  // (since C++26)
```

The template specialization of `std::hash` for `std::chrono::time_point` allows
users to obtain hashes of objects of type std::chrono::time_point<Clock,
Duration>. This specialization is enabled if and only if both std::hash<Clock>
and std::hash<Duration> are enabled.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_chrono` | 202306L | (C++26) | Hashing support for `std::chrono`
      value classes

### Example

```cpp
#include <chrono>
#include <cstddef>
#include <iostream>
#include <string>
#include <thread>
#include <unordered_map>
using system_clock_tp = std::chrono::time_point<std::chrono::system_clock>;

#if __cpp_lib_chrono < 202306L
// custom specialization of std::hash can be injected in namespace std
template<>
struct std::hash<system_clock_tp>
{
    std::size_t operator()(const system_clock_tp& d) const noexcept
    {
        return d.time_since_epoch().count();
    }
};
#endif

int main()
{
    using namespace std::chrono_literals;

    std::unordered_map<system_clock_tp, std::string> log;

    for (int i{}; i != 4; ++i)
    {
        std::this_thread::sleep_for(100ms);
        log[std::chrono::system_clock::now()] = "event #" + std::to_string(i);
    }

    for (auto const& [time, message] : log)
        std::cout << '[' << time << "], message: " << message << '\n';
}
```

Possible output:

```text
[2023-08-31 21:42:26.100842469], message: event #3
[2023-08-31 21:42:26.000441887], message: event #2
[2023-08-31 21:42:25.900354894], message: event #1
[2023-08-31 21:42:25.800025900], message: event #0
```

### See also

- **hash (C++11)** — hash function object (class template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/time_point/hash*
