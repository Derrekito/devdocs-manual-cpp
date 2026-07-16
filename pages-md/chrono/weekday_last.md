# std::chrono::weekday_last

```cpp
class weekday_last;  // (since C++20)
```

The class `weekday_last` represents the last weekday of some month.

`weekday_last` is a TriviallyCopyable StandardLayoutType.

### Member functions

- **(constructor)** — constructs a `weekday_last` object (public member
  function)
- **weekday** — accesses the stored `weekday` (public member function)
- **ok** — checks if the stored `weekday` is valid (public member function)

### Nonmember functions

- **operator== (C++20)** — compares two `weekday_last` values (function)
- **operator<< (C++20)** — outputs a `weekday_last` into a stream (function
  template)

### Helper classes

- **std::formatter<std::chrono::weekday_last> (C++20)** — formatting support for
  `weekday_last` (class template specialization)
- **std::hash<std::chrono::weekday_last> (C++26)** — hash support for
  **`std::chrono::weekday_last`** (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/weekday_last*
