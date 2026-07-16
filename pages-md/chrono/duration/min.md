# std::chrono::duration<Rep,Period>::min

```cpp
static constexpr duration min();  // (until C++20)
static constexpr duration min() noexcept;  // (since C++20)
```

Returns a duration with the lowest possible value.

If the representation `rep` of the duration requires some other implementation
to return a minimum-length duration, `std::chrono::duration_values` can be
specialized to return the desired value.

### Parameters

(none)

### Return value

`duration(std::chrono::duration_values<rep>::min())`

### See also

- **zero [static]** — returns the special duration value zero (public static
  member function)
- **max [static]** — returns the special duration value max (public static
  member function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/min*
