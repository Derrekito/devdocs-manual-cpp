# std::chrono::hh_mm_ss<Duration>::hh_mm_ss

```cpp
constexpr hh_mm_ss() noexcept : hh_mm_ss{Duration::zero()} {}  // (1)
constexpr explicit hh_mm_ss( Duration d );  // (2)
```

Constructs a `hh_mm_ss` object.

1) Constructs a `hh_mm_ss` object corresponding to `Duration::zero()`.
2) Constructs a `hh_mm_ss` object corresponding to `d`:

- `is_negative()` returns `d < Duration::zero()`.
- `hours()` returns `std::chrono::duration_cast<std::chrono::hours>(abs(d))`.
- `minutes()` returns `std::chrono::duration_cast<std::chrono::minutes>(abs(d) -
  hours())`.
- `seconds()` returns `std::chrono::duration_cast<std::chrono::seconds>(abs(d) -
  hours() - minutes())`.
- `subseconds()` returns `abs(d) - hours() - minutes() - seconds()` if
  `std::chrono::treat_as_floating_point_v<precision::rep>` is `true`; otherwise
  it returns `std::chrono::duration_cast<precision>(abs(d) - hours() - minutes()
  - seconds())`.

### Parameters

- **d** — the duration to be broken down

### Example

---
*Source: https://en.cppreference.com/w/cpp/chrono/hh_mm_ss/hh_mm_ss*
