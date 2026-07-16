# std::chrono::zoned_time

```cpp
template<
    class Duration,
    class TimeZonePtr = const std::chrono::time_zone*
> class zoned_time;  // (since C++20)
using zoned_seconds = std::chrono::zoned_time<std::chrono::seconds>;  // (since C++20)
```

The class `zoned_time` represents a logical pairing of a time zone and a
`std::chrono::time_point` whose resolution is `Duration`.

An invariant of `zoned_time` is that it always refers to a valid time zone and
represents an existing and unambiguous time point in that time zone. Consistent
with this invariant, `zoned_time` has no move constructor or move assignment
operator; attempts to move a `zoned_time` will perform a copy.

The program is ill-formed if `Duration` is not a specialization of
`std::chrono::duration`.

The template parameter `TimeZonePtr` allows users to supply their own time zone
pointer types and further customize the behavior of `zoned_time` via
`std::chrono::zoned_traits`. Custom time zone types need not support all the
operations supported by `std::chrono::time_zone`, only those used by the
functions actually called on the `zoned_time`.

`TimeZonePtr` must be MoveConstructible. Move-only `TimeZonePtr`s are allowed
but difficult to use, as the `zoned_time` will be immovable and it is not
possible to access the stored `TimeZonePtr`.

### Member types

- **`duration`** — std::common_type_t<Duration, std::chrono::seconds>

### Member functions

- **(constructor)** — constructs a `zoned_time` (public member function)
- **operator=** — assigns value to a `zoned_time` (public member function)
- **get_time_zone** — obtains a copy of the time zone pointer (public member
  function)
- **operator local_timeget_local_time** — obtains the stored time point as a
  `local_time` (public member function)
- **operator sys_timeget_sys_time** — obtains the stored time point as a
  `sys_time` (public member function)
- **get_info** — obtain information about the time zone at the stored time point
  (public member function)

### Non-member functions

- **operator== (C++20)** — compares two `zoned_time` values (function template)
- **operator<< (C++20)** — outputs a `zoned_time` into a stream (function
  template)

### Helper classes

- **std::formatter<std::chrono::zoned_time> (C++20)** — formatting support for
  `zoned_time` (class template specialization)
- **std::hash<std::chrono::zoned_time> (C++26)** — hash support for
  **`std::chrono::zoned_time`** (class template specialization)

### Deduction guides

### Example

```cpp
#include <algorithm>
#include <chrono>
#include <iomanip>
#include <iostream>
#include <stdexcept>
#include <string_view>

int main()
{
    constexpr std::string_view locations[] =
    {
        "Africa/Casablanca",   "America/Argentina/Buenos_Aires",
        "America/Barbados",    "America/Indiana/Petersburg",
        "America/Tarasco_Bar", "Antarctica/Casey",
        "Antarctica/Vostok",   "Asia/Magadan",
        "Asia/Manila",         "Asia/Shanghai",
        "Asia/Tokyo",          "Atlantic/Bermuda",
        "Australia/Darwin",    "Europe/Isle_of_Man",
        "Europe/Laputa",       "Indian/Christmas",
        "Indian/Cocos",        "Pacific/Galapagos",
    };

    constexpr auto width = std::ranges::max_element(locations, {},
        [](const auto& s){ return s.length(); })->length();

    for (const auto location : locations)
        try
        {
            // may throw if 'location' is not in the time zone database
            const std::chrono::zoned_time zt{location, std::chrono::system_clock::now()};
            std::cout << std::setw(width) << location << " - Zoned Time: " << zt << '\n';
        }
        catch (std::runtime_error& ex)
        {
            std::cout << "Error: " << ex.what() << '\n';
        }
}
```

Possible output:

```text
             Africa/Casablanca - Zoned Time: 2023-06-29 20:58:34.697449319 +01
America/Argentina/Buenos_Aires - Zoned Time: 2023-06-29 16:58:34.709957354 -03
              America/Barbados - Zoned Time: 2023-06-29 15:58:34.709977888 AST
    America/Indiana/Petersburg - Zoned Time: 2023-06-29 15:58:34.709998072 EDT
Error: tzdb: cannot locate zone: America/Tarasco_Bar
              Antarctica/Casey - Zoned Time: 2023-06-30 06:58:34.710093685 +11
             Antarctica/Vostok - Zoned Time: 2023-06-30 01:58:34.710107932 +06
                  Asia/Magadan - Zoned Time: 2023-06-30 06:58:34.710121831 +11
                   Asia/Manila - Zoned Time: 2023-06-30 03:58:34.710134751 PST
                 Asia/Shanghai - Zoned Time: 2023-06-30 03:58:34.710153259 CST
                    Asia/Tokyo - Zoned Time: 2023-06-30 04:58:34.710172815 JST
              Atlantic/Bermuda - Zoned Time: 2023-06-29 16:58:34.710191043 ADT
              Australia/Darwin - Zoned Time: 2023-06-30 05:28:34.710236720 ACST
            Europe/Isle_of_Man - Zoned Time: 2023-06-29 20:58:34.710256834 BST
Error: tzdb: cannot locate zone: Europe/Laputa
              Indian/Christmas - Zoned Time: 2023-06-30 02:58:34.710360409 +07
                  Indian/Cocos - Zoned Time: 2023-06-30 02:28:34.710377520 +0630
             Pacific/Galapagos - Zoned Time: 2023-06-29 13:58:34.710389952 -06
```

### See also

- **time_zone (C++20)** — represents a time zone (class)

---
*Source: https://en.cppreference.com/w/cpp/chrono/zoned_time*
