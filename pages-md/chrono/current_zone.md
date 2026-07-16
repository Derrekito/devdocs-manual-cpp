# std::chrono::current_zone

```cpp
const std::chrono::time_zone* current_zone();  // (since C++20)
```

Convenience function for obtaining local time zone from the time zone database.
Equivalent to `std::chrono::get_tzdb().current_zone()`.

### Exceptions

`std::runtime_error` if this is the first reference to the time zone database
and the time zone database cannot be initialized.

### Notes

A call to this function that is the first reference to the time zone database
will cause it to be initialized.

### Example

```cpp
#include <chrono>
#include <iostream>

int main() {
    const std::chrono::zoned_time cur_time{ std::chrono::current_zone(),
                                            std::chrono::system_clock::now() };
    std::cout << cur_time << '\n';
}
```

Possible output:

```text
2021-09-13 19:46:42.249182012 MAGT
```

### See also

- **current_zone** — return the local time zone (public member function of
  `std::chrono::tzdb`)
- **get_tzdbget_tzdb_listreload_tzdbremote_version (C++20)** — accesses and
  controls the global time zone database information (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/current_zone*
