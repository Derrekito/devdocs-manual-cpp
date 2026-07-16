# std::chrono::month_weekday::month_weekday

```cpp
constexpr month_weekday( const std::chrono::month& m,
                         const std::chrono::weekday_indexed& wdi ) noexcept;  // (since C++20)
```

Constructs a `month_weekday` object that stores the `month` `m` and the
`weekday_indexed` `wdi`.

### Notes

A more convenient way to construct a `month_weekday` is with `operator/`, e.g.,
`std::chrono::April/std::chrono::Sunday[2]`.

### See also

- **operator/ (C++20)** — conventional syntax for Gregorian calendar date
  creation (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_weekday/month_weekday*
