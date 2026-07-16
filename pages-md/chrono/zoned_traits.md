# std::chrono::zoned_traits

```cpp
template < class TimeZonePtr >
struct zoned_traits { };  // (1) (since C++20)
template <>
struct zoned_traits<const std::chrono::time_zone*>;  // (2) (since C++20)
```

The class `zoned_traits` provides a way to customize the behavior of the
constructors of `std::chrono::zoned_time` with custom time zone pointer types.
In particular, it allows such types to specify the default time zone to use and
the mapping of a time zone's name to the corresponding time zone pointer. It is
acceptable for custom time zone pointer types to not support either operation,
in which case the corresponding constructors of `zoned_time` will not
participate in overload resolution.

The primary template is empty. A specialization is provided for `const
std::chrono::time_zone*`, the default time zone pointer type.

### Member functions

## std::chrono::zoned_traits<const std::chrono::time_zone*>::default_zone

```cpp
static const std::chrono::time_zone* default_zone();
```

Returns a time zone pointer for the default time zone (UTC).

### Return value

`std::chrono::locate_zone("UTC")`.

## std::chrono::zoned_traits<const std::chrono::time_zone*>::locate_zone

```cpp
static const std::chrono::time_zone* locate_zone(std::string_view name);
```

Returns the time zone pointer for the time zone designated by `name`.

### Return value

`std::chrono::locate_zone(name)`.

---
*Source: https://en.cppreference.com/w/cpp/chrono/zoned_traits*
