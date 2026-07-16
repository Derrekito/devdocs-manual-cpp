# std::chrono::day::ok

```cpp
constexpr bool ok() const noexcept;  // (since C++20)
```

Checks if the day value stored in `*this` is in the valid range, i.e.,
`[``1``,``31``]`.

### Return value

`true` if the day value stored in `*this` is in the range `[``1``,``31``]`.
Otherwise `false`.

### Example

```cpp
#include <chrono>
using namespace std::chrono_literals;

constexpr std::chrono::day d0{00};
constexpr std::chrono::day d1{13};
constexpr std::chrono::day d2{42};

static_assert
(
    d0 == 0d && !d0.ok() &&
    d1 == 13d && d1.ok() &&
    d2 == 42d && !d2.ok()
);

int main() {}
```

### See also

- **operator unsigned** — retrieves the stored day value (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/day/ok*
