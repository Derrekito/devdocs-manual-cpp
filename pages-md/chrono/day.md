# std::chrono::day

```cpp
class day;  // (since C++20)
```

The class `day` represents a day in a month. Its normal range is `[`1`,`31`]`,
but it may hold any number in `[`​0​`,`255`]`.

`day` is a TriviallyCopyable StandardLayoutType.

### Member functions

- **(constructor)** — constructs a `day` (public member function)
- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the day (public member function)
- **operator+=operator-=** — adds or subtracts a number of days (public member
  function)
- **operator unsigned** — retrieves the stored day value (public member
  function)
- **ok** — checks if the stored day value is in the normal range (public member
  function)

### Nonmember functions

- **operator==operator<=> (C++20)** — compares two `day` values (function)
- **operator+operator- (C++20)** — adds or subtracts a number of days and a
  `day`, or find the difference between two `day`s (function)
- **operator<< (C++20)** — outputs a `day` into a stream (function template)
- **from_stream (C++20)** — parses a `day` from a stream according to the
  provided format (function template)

### Helper classes

- **std::formatter<std::chrono::day> (C++20)** — formatting support for `day`
  (class template specialization)
- **std::hash<std::chrono::day> (C++26)** — hash support for
  **`std::chrono::day`** (class template specialization)

### Literals

- **operator""d (C++20)** — a `std::chrono::day` literal representing a day of a
  month (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/day*
