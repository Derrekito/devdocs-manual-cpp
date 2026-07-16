# std::chrono::month_weekday_last::month_weekday_last

```cpp
constexpr month_weekday_last( const std::chrono::month& m,
                              const std::chrono::weekday_last& wdl ) noexcept;  // (since C++20)
```

Constructs a `month_weekday_last` object that stores the `month` `m` and the
`weekday_last` `wdl`.

### Notes

A more convenient way to construct a `month_weekday_last` is with `operator/`,
e.g., `std::chrono::April/std::chrono::Sunday[std::chrono::last]`.

### See also

- **operator/ (C++20)** — conventional syntax for Gregorian calendar date
  creation (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_weekday_last/month_weekday_last*
