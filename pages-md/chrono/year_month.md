# std::chrono::year_month

```cpp
class year_month;  // (since C++20)
```

The class `year_month` represents a specific month of a specific year, but with
an unspecified day. It is a field-based time point, with a resolution of
`std::chrono::months`.

`year_month` is a TriviallyCopyable StandardLayoutType.

### Member functions

- **(constructor)** — constructs a `year_month` (public member function)
- **yearmonth** — accesses the stored year and month (public member function)
- **operator+=operator-=** — modifies the `year_month` by some number of months
  or years (public member function)
- **ok** — checks if this `year_month` is valid (public member function)

### Nonmember functions

- **operator==operator<=> (C++20)** — compares two `year_month` values
  (function)
- **operator+operator- (C++20)** — performs arithmetic on `year_month`
  (function)
- **operator<< (C++20)** — outputs a `year_month` into a stream (function
  template)
- **from_stream (C++20)** — parses a `year_month` from a stream according to the
  provided format (function template)

### Helper classes

- **std::formatter<std::chrono::year_month> (C++20)** — formatting support for
  `year_month` (class template specialization)
- **std::hash<std::chrono::year_month> (C++26)** — hash support for
  **`std::chrono::year_month`** (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month*
