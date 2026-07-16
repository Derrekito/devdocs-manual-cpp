# std::chrono::year::ok

```cpp
constexpr bool ok() const noexcept;  // (since C++20)
```

Checks if the year value stored in `*this` is in the valid range, i.e.,
`[``-32767``,``32767``]`.

### Return value

`true` if the year value stored in `*this` is in the range
`[``-32767``,``32767``]`. Otherwise `false`.

### Possible implementation

See the implementations in libstdc++, libc++, and Howard Hinnant's date.h.

```cpp
class Year
{
    short year_;   // exposition-only

public:

    bool ok() const noexcept { return year_ != std::numeric_limits<short>::min(); }

    /*...*/
};
```

### Example

```cpp
#include <chrono>
#include <iomanip>
#include <iostream>

int main()
{
    std::cout << "input year │ internal value │ ok()\n" << std::boolalpha;

    for (const int i : {2020, 0x8000, 0x8001, 0xFFFF, 0x18000})
    {
        const std::chrono::year y{i};
        std::cout << std::setw(10) << i << " │ "
                  << std::setw(14) << static_cast<int>(y) << " │ "
                  << y.ok() << '\n';
    }
}
```

Possible output:

```text
input year │ internal value │ ok()
      2020 │           2020 │ true
     32768 │         -32768 │ false
     32769 │         -32767 │ true
     65535 │             -1 │ true
     98304 │         -32768 │ false
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/year/ok*
