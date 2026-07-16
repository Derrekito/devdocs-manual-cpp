# std::chrono::year_month_weekday::year_month_weekday

```cpp
year_month_weekday() = default;  // (1) (since C++20)
constexpr year_month_weekday( const std::chrono::year& y,
                              const std::chrono::month& m,
                              const std::chrono::weekday_indexed& wdi ) noexcept;  // (2) (since C++20)
constexpr year_month_weekday( const std::chrono::sys_days& dp ) noexcept;  // (3) (since C++20)
constexpr explicit year_month_weekday( const std::chrono::local_days& dp ) noexcept;  // (4) (since C++20)
```

Constructs a `year_month_weekday` object.

1) Default constructor leaves the fields uninitialized.

2) Constructs a `year_month_weekday` object storing the year `y`, the month `m`,
   the weekday `wdi.weekday()` and the weekday index `wdi.index()`.

3) Constructs a `year_month_weekday` object corresponding to the date
   represented by `dp`. For any `year_month_weekday` object storing a valid
   date, converting it to `sys_days` and back yields the same value. This
   constructor defines an implicit conversion from `sys_days` to
   `year_month_weekday`.

4) Constructs a `year_month_weekday` object corresponding to the date
   represented by `dp`. Equivalent to
   `year_month_weekday(sys_days(dp.time_since_epoch()))`.

### Notes

A `year_month_weekday` can also be created by combining one of the partial-date
types `std::chrono::year_month` and `std::chrono::month_weekday` with the
missing component (indexed weekday and year, respectively) using `operator/`.

### See also

- **operator/ (C++20)** — conventional syntax for Gregorian calendar date
  creation (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_weekday/year_month_weekday*
