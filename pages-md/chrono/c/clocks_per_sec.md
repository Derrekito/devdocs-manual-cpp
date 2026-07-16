# CLOCKS_PER_SEC

```cpp
#define CLOCKS_PER_SEC /*implementation defined*/
```

Expands to an expression (not necessarily a compile-time constant) of type
`std::clock_t` equal to the number of clock ticks per second, as returned by
`std::clock()`.

### Notes

POSIX defines `CLOCKS_PER_SEC` as one million, regardless of the actual
precision of `std::clock()`.

### Example

```cpp
#include <clocale>
#include <ctime>
#include <iostream>

int main()
{
    const std::clock_t cps{CLOCKS_PER_SEC};
    std::cout.imbue(std::locale("en_US.utf8"));
    std::cout << cps << '\n';
}
```

Possible output:

```text
1,000,000
```

### See also

- **clock** — returns raw processor clock time since the program is started
  (function)
- **clock_t** — process running time (typedef)

**C documentation for `CLOCKS_PER_SEC`**

---
*Source: https://en.cppreference.com/w/cpp/chrono/c/CLOCKS_PER_SEC*
