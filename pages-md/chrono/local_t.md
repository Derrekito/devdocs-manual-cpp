# std::chrono::local_t

```cpp
struct local_t {};  // (since C++20)
```

The class `local_t` is a pseudo-clock that is used as the first template
argument to `std::chrono::time_point` to indicate that the time point represents
local time with respect of a not-yet-specified time zone. `local_time` supports
streaming and the full set of time point arithmetic.

### Time point family

```cpp
Defined in namespace std::chrono
template<class Duration>
using local_time = std::chrono::time_point<std::chrono::local_t, Duration>;  // (since C++20)
using local_seconds = local_time<std::chrono::seconds>;  // (since C++20)
using local_days = local_time<std::chrono::days>;  // (since C++20)
```

- **operator<<(std::chrono::local_time) (C++20)** — performs stream output on a
  `local_time` (function template)
- **from_stream(std::chrono::local_time) (C++20)** — parses a `local_time` from
  a stream according to the provided format (function template)
- **std::formatter<std::chrono::local_time> (C++20)** — formatting support for
  `local_time` (class template specialization)

### See also

- **zoned_time (C++20)** — represents a time zone and a time point (class)

---
*Source: https://en.cppreference.com/w/cpp/chrono/local_t*
