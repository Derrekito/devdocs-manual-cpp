# std::chrono::tzdb

```cpp
struct tzdb;  // (since C++20)
```

The class `tzdb` represents a copy of the IANA time zone database. Users cannot
construct a `tzdb` and can only obtain read-only access to one via the free
functions `std::chrono::get_tzdb_list` and `std::chrono::get_tzdb`.

### Member objects

- **`version`** — A `std::string` that contains the version of the database
- **`zones`** — A sorted `std::vector<std::chrono::time_zone>` containing
  description of time zones
- **`links`** — A sorted `std::vector<std::chrono::time_zone_link>` containing
  description of alternative names of time zones (links)
- **`leap_seconds`** — A sorted `std::vector<std::chrono::leap_second>`
  containing description of leap seconds

### Member functions

- **locate_zone** — locate a time zone with the given name (public member
  function)
- **current_zone** — return the local time zone (public member function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/tzdb*
