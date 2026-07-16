# std::chrono::month_weekday::ok

```cpp
constexpr bool ok() const noexcept;  // (since C++20)
```

Checks if the contained `month` and `weekday_indexed` objects are valid.

### Return value

`month().ok() && weekday_indexed().ok()`

### Example

```cpp
#include <cassert>
#include <chrono>

int main()
{
    auto mwdi{std::chrono::March/std::chrono::Friday[1]};
    assert(mwdi.ok());

    mwdi = {std::chrono::month(17)/std::chrono::Friday[1]};
    assert(not mwdi.ok());

    mwdi = {std::chrono::March/std::chrono::Friday[-4]};
    assert(not mwdi.ok());
}
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_weekday/ok*
