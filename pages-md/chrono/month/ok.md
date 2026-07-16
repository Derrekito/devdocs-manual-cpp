# std::chrono::month::ok

```cpp
constexpr bool ok() const noexcept;  // (since C++20)
```

Checks if the month value stored in `*this` is in the valid range, i.e.,
`[``1``,``12``]`.

### Return value

`true` if the month value stored in `*this` is in the range `[``1``,``12``]`.
Otherwise `false`.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    for (const unsigned mm : {6u, 0u, 16U})
    {
        std::cout << mm << ": ";
        const std::chrono::month m{mm};
        m.ok() ? std::cout << "month is valid\n"
               : std::cout << "month is invalid\n";
    }
}
```

Output:

```text
6: month is valid
0: month is invalid
16: month is invalid
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/month/ok*
