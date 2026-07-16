# std::chrono::time_zone_link::name, std::chrono::time_zone_link::target

```cpp
std::string_view name() const noexcept;  // (1)
std::string_view target() const noexcept;  // (2)
```

Accesses the name and target of this `time_zone_link` object.

### Return value

1) The alternative name this `time_zone_link` object represents.

2) The name of the `std::chrono::time_zone` for which this `time_zone_link`
   object provides an alternative name.

---
*Source: https://en.cppreference.com/w/cpp/chrono/time_zone_link/accessors*
