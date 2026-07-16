# std::chrono::weekday_indexed::ok

```cpp
constexpr bool ok() const noexcept;  // (since C++20)
```

Checks if the weekday object and the index stored in `*this` are both valid.

### Return value

`true` if `weekday().ok() == true` and `index()` is in the range
`[``1``,``5``]`. Otherwise `false`.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    std::cout << std::boolalpha;

    std::chrono::weekday_indexed wdi{std::chrono::Tuesday[2]};
    std::cout << (wdi.ok()) << ' ';
    wdi = {std::chrono::weekday(42)[2]};
    std::cout << (wdi.ok()) << ' ';
    wdi = {std::chrono::Tuesday[-4]};
    std::cout << (wdi.ok()) << '\n';
}
```

Output:

```text
true false false
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/weekday_indexed/ok*
