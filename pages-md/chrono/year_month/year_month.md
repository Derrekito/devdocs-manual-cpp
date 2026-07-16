# std::chrono::year_month::year_month

```cpp
year_month() = default;  // (1) (since C++20)
constexpr year_month( const std::chrono::year& y,
                      const std::chrono::month& m ) noexcept;  // (2) (since C++20)
```

Constructs a `year_month` object.

1) Default constructor leaves the year and month uninitialized.

2) Constructs a `year_month` object storing the year `y` and the month `m`.

### Notes

A more convenient way to construct a `year_month` is with `operator/`, e.g.,
`2007y/std::chrono::April`.

### See also

- **operator/ (C++20)** — conventional syntax for Gregorian calendar date
  creation (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month/year_month*
