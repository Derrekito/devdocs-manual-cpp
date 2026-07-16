# std::chrono::month_day_last

```cpp
class month_day_last;  // (since C++20)
```

The class `month_day_last` represents the last day of a specific month, of some
yet to be specified year.

`month_day_last` is a TriviallyCopyable StandardLayoutType.

### Member functions

- **(constructor)** — constructs a `month_day_last` (public member function)
- **month** — accesses the stored `month` (public member function)
- **ok** — checks if the stored `month` is valid (public member function)

### Nonmember functions

- **operator==operator<=> (C++20)** — compares two `month_day_last` values
  (function)
- **operator<< (C++20)** — outputs a `month_day_last` into a stream (function
  template)

### Helper classes

- **std::formatter<std::chrono::month_day_last> (C++20)** — formatting support
  for `month_day_last` (class template specialization)
- **std::hash<std::chrono::month_day_last> (C++26)** — hash support for
  **`std::chrono::month_day_last`** (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_day_last*
