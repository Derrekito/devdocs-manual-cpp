# std::chrono::year

```cpp
class year;  // (since C++20)
```

The class `year` represents a year in the proleptic Gregorian calendar. Its
range is `[`-32767`,`32767`]`.

`year` is a TriviallyCopyable StandardLayoutType.

### Member functions

- **(constructor)** — constructs a `year` (public member function)
- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the year (public member function)
- **operator+=operator-=** — adds or subtracts a number of years from a `year`
  (public member function)
- **operator+operator-** — applies unary operators to `year` (public member
  function)
- **is_leap** — determines if the year is a leap year (public member function)
- **operator int** — retrieve the year value (public member function)
- **ok** — checks if the stored year value is valid (public member function)
- **min [static]** — returns the smallest possible year value (public static
  member function)
- **max [static]** — returns the largest possible `year` (public static member
  function)

### Non-member functions

- **operator==operator<=> (C++20)** — compares two `year` values (function)
- **operator+operator- (C++20)** — performs arithmetic on `year`s (function)
- **operator<< (C++20)** — outputs a `year` into a stream (function template)
- **from_stream (C++20)** — parses a `year` from a stream according to the
  provided format (function template)

### Helper classes

- **std::formatter<std::chrono::year> (C++20)** — formatting support for `year`
  (class template specialization)
- **std::hash<std::chrono::year> (C++26)** — hash support for
  **`std::chrono::year`** (class template specialization)

### Literals

- **operator""y (C++20)** — a `std::chrono::year` literal representing a
  particular year (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year*
