# std::chrono::clock_cast

```cpp
template< class Dest, class Source, class Duration >
auto clock_cast( const std::chrono::time_point<Source, Duration>& t );  // (since C++20)
```

Converts the time point `t` of a clock `Source` to an equivalent time point of
the clock `Dest`, using `std::chrono::system_clock` and/or
`std::chrono::utc_clock` as intermediaries if necessary.

- If the expression `std::chrono::clock_time_conversion<Dest, Source>{}(t)` is
  well-formed, returns the result of that expression.
- Otherwise, if at least one of the following two expressions are well-formed,
  then

   1) `std::chrono::clock_time_conversion<Dest, std::chrono::system_clock>{}(
      std::chrono::clock_time_conversion<std::chrono::system_clock,
      Source>{}(t))`

   2) `std::chrono::clock_time_conversion<Dest, std::chrono::utc_clock>{}(
      std::chrono::clock_time_conversion<std::chrono::utc_clock, Source>{}(t))`

- Otherwise, if at least one of the following two expressions are well-formed,
  then

   1) `std::chrono::clock_time_conversion<Dest, std::chrono::utc_clock>{}(
      std::chrono::clock_time_conversion<std::chrono::utc_clock,
      std::chrono::system_clock>{}( std::chrono::clock_time_conversion
      <std::chrono::system_clock, Source>{}(t)))`

   2) `std::chrono::clock_time_conversion<Dest, std::chrono::system_clock>{}(
      std::chrono::clock_time_conversion<std::chrono::system_clock,
      std::chrono::utc_clock>{}( std::chrono::clock_time_conversion
      <std::chrono::utc_clock, Source>{}(t)))`

- Otherwise, this function does not participate in overload resolution.

### Return value

The result of the conversion, determined as described above.

### Example

### See also

- **clock_time_conversion (C++20)** — traits class defining how to convert time
  points of one clock to another (class template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/clock_cast*
