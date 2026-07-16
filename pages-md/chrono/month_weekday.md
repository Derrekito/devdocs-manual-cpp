# std::chrono::month_weekday

```cpp
header  // (since C++20)
```

The class `month_weekday` represents the nth weekday of a specific month, of
some yet to be specified year.

`month_weekday` is a TriviallyCopyable StandardLayoutType.

### Member functions

- **(constructor)** — constructs a `month_weekday` (public member function)
- **monthweekday_indexed** — retrieves the stored `month` and `weekday_indexed`
  values (public member function)
- **ok** — checks whether the contained `month` and `weekday_indexed` are valid
  (public member function)

### Nonmember functions

- **operator== (C++20)** — compares two `month_weekday` values (function)
- **operator<< (C++20)** — outputs a `month_weekday` into a stream (function
  template)

### Helper classes

- **std::formatter<std::chrono::month_weekday> (C++20)** — formatting support
  for `month_weekday` (class template specialization)
- **std::hash<std::chrono::month_weekday> (C++26)** — hash support for
  **`std::chrono::month_weekday`** (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_weekday*
