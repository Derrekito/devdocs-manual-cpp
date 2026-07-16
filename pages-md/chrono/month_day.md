# std::chrono::month_day

```cpp
class month_day;  // (since C++20)
```

The class `month_day` represents a specific day of a specific month, of some yet
to be specified year.

`month_day` is a TriviallyCopyable StandardLayoutType.

### Member functions

- **(constructor)** — constructs a `month_day` (public member function)
- **monthday** — retrieve the stored `month` and `day` values (public member
  function)
- **ok** — checks if the `month_day` is valid (public member function)

### Nonmember functions

- **operator==operator<=> (C++20)** — compares two `month_day` values (function)
- **operator<< (C++20)** — outputs a `month_day` into a stream (function
  template)
- **from_stream (C++20)** — parses a `month_day` from a stream according to the
  provided format (function template)

### Helper classes

- **std::formatter<std::chrono::month_day> (C++20)** — formatting support for
  `month_day` (class template specialization)
- **std::hash<std::chrono::month_day> (C++26)** — hash support for
  **`std::chrono::month_day`** (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_day*
