# std::chrono::year_month_weekday

```cpp
class year_month_weekday;  // (since C++20)
```

The class `year_month_weekday` represents the nth weekday of a specific year and
month. It is a field-based time point, with a resolution of `std::chrono::days`.
`std::chrono::years`- and `std::chrono::months`-oriented arithmetic are
supported directly. An implicit conversion to and from `std::chrono::sys_days`
allows `std::chrono::days`-oriented arithmetic to be performed efficiently.

`year_month_weekday` is a TriviallyCopyable StandardLayoutType.

### Member functions

- **(constructor)** — constructs a `year_month_weekday` (public member function)
- **operator+=operator-=** — modifies the time point by some number of months or
  years (public member function)
- **yearmonthweekdayindexweekday_indexed** — accesses the fields of this object
  (public member function)
- **operator sys_daysoperator local_days** — converts to a
  `std::chrono::time_point` (public member function)
- **ok** — checks whether this object represents a valid date (public member
  function)

### Nonmember functions

- **operator== (C++20)** — compares two `year_month_weekday` values (function)
- **operator+operator- (C++20)** — adds or subtracts a `year_month_weekday` and
  some number of years or months (function)
- **operator<< (C++20)** — outputs a `year_month_weekday` into a stream
  (function template)

### Helper classes

- **std::formatter<std::chrono::year_month_weekday> (C++20)** — formatting
  support for `year_month_weekday` (class template specialization)
- **std::hash<std::chrono::year_month_weekday> (C++26)** — hash support for
  **`std::chrono::year_month_weekday`** (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_weekday*
