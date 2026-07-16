# std::chrono::month

```cpp
class month;  // (since C++20)
inline constexpr std::chrono::month January{1};  // (since C++20)
inline constexpr std::chrono::month February{2};  // (since C++20)
inline constexpr std::chrono::month March{3};  // (since C++20)
inline constexpr std::chrono::month April{4};  // (since C++20)
inline constexpr std::chrono::month May{5};  // (since C++20)
inline constexpr std::chrono::month June{6};  // (since C++20)
inline constexpr std::chrono::month July{7};  // (since C++20)
inline constexpr std::chrono::month August{8};  // (since C++20)
inline constexpr std::chrono::month September{9};  // (since C++20)
inline constexpr std::chrono::month October{10};  // (since C++20)
inline constexpr std::chrono::month November{11};  // (since C++20)
inline constexpr std::chrono::month December{12};  // (since C++20)
```

The class `month` represents a month in a year. Its normal range is
`[`1`,`12`]`, but it may hold any number in `[`​0​`,`255`]`. Twelve named
constants are predefined in the `std::chrono` namespace for the twelve months of
the year.

`month` is a TriviallyCopyable StandardLayoutType.

### Member functions

- **(constructor)** — constructs a `month` (public member function)
- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the month (public member function)
- **operator+=operator-=** — adds or subtracts a number of months (public member
  function)
- **operator unsigned** — retrieves the stored month value (public member
  function)
- **ok** — checks if the stored month value is in the normal range (public
  member function)

### Nonmember functions

- **operator==operator<=> (C++20)** — compares two `month` values (function)
- **operator+operator- (C++20)** — performs arithmetic on `month`s (function)
- **operator<< (C++20)** — outputs a `month` into a stream (function template)
- **from_stream (C++20)** — parses a `month` from a stream according to the
  provided format (function template)

### Helper classes

- **std::formatter<std::chrono::month> (C++20)** — formatting support for
  `month` (class template specialization)
- **std::hash<std::chrono::month> (C++26)** — hash support for
  **`std::chrono::month`** (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/month*
