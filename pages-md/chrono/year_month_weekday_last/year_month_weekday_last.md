# std::chrono::year_month_weekday_last::year_month_weekday_last

```cpp
constexpr
year_month_weekday_last( const std::chrono::year& y, const std::chrono::month& m,
                         const std::chrono::weekday_last& wdl ) noexcept;  // (since C++20)
```

Constructs a `year_month_weekday_last` object storing the year `y`, the month
`m`, and the weekday `wdl.weekday()`. The constructed object represents the last
weekday of that year and month.

### Notes

A `year_month_weekday_last` can also be created by combining one of the
partial-date types `std::chrono::year_month` and
`std::chrono::month_weekday_last` with the missing component (`weekday_last` and
`year`, respectively) using `operator/`.

### See also

- **operator/ (C++20)** — conventional syntax for Gregorian calendar date
  creation (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_weekday_last/year_month_weekday_last*
