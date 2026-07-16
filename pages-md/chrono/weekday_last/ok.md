# std::chrono::weekday_last::ok

```cpp
constexpr bool ok() const noexcept;  // (since C++20)
```

Checks if the weekday object stored in `*this` is valid.

### Return value

`weekday().ok()`

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    std::cout << std::boolalpha;

    auto wdl{std::chrono::Tuesday[std::chrono::last]};
    std::cout << (wdl.ok()) << ' ';

    wdl = {std::chrono::weekday(42)[std::chrono::last]};
    std::cout << (wdl.ok()) << '\n';
}
```

Output:

```text
true false
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/weekday_last/ok*
