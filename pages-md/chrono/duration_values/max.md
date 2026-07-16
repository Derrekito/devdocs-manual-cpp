# std::chrono::duration_values<Rep>::max

```cpp
static constexpr Rep max();  // (since C++11) (until C++20)
static constexpr Rep max() noexcept;  // (since C++20)
```

Returns the largest possible representation.

### Parameters

(none)

### Return value

`std::numeric_limits<Rep>::max()`

### See also

- **max [static]** — returns the special duration value max (public static
  member function of `std::chrono::duration<Rep,Period>`)
- **zero [static]** — returns a zero-length representation (public static member
  function)
- **min [static]** — returns the smallest possible representation (public static
  member function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration_values/max*
