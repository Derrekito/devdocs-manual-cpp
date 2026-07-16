# std::chrono::hh_mm_ss

```cpp
template< class Duration >
class hh_mm_ss;  // (since C++20)
```

The class template `hh_mm_ss` splits a `std::chrono::duration` into a "broken
down" time such as *hours*:*minutes*:*seconds*, with the precision of the split
determined by the `Duration` template parameter. It is primarily a formatting
tool.

`Duration` must be a specialization of `std::chrono::duration`, otherwise the
program is ill-formed.

### Member constants

- **constexpr unsigned fractional_width [static]** — the smallest possible
  integer in the range `[`​0​`,`18`]` such that `precision` (see below) will
  exactly represent the value of Duration{1}, or 6 if there's no such integer
  (public static member constant)

### Member types

- **`precision`** — `std::chrono::duration<std::common_type_t<Duration::rep,
  std::chrono::seconds::rep>, std::ratio<1, 10fractional_width>>`

### Member functions

- **(constructor)** — constructs a `hh_mm_ss` (public member function)
- **is_negativehoursminutessecondssubseconds** — obtains components of the
  broken-down time (public member function)
- **operator precisionto_duration** — obtains the stored `std::chrono::duration`
  (public member function)

### Non-member functions

- **operator<< (C++20)** — outputs a `hh_mm_ss` into a stream (function
  template)

### Helper classes

- **std::formatter<std::chrono::hh_mm_ss> (C++20)** — formatting support for
  `hh_mm_ss` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/hh_mm_ss*
