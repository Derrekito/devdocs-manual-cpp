# std::hash<std::chrono::duration>

```cpp
template< class Rep, class Period >
struct hash<std::chrono::duration<Rep, Period>>;  // (since C++26)
```

The template specialization of `std::hash` for `std::chrono::duration` allows
users to obtain hashes of objects of type std::chrono::duration<Rep, Period>.
This specialization is enabled if and only if both std::hash<Rep> and
std::hash<Period> are enabled.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_chrono` | 202306L | (C++26) | Hashing support for `std::chrono`
      value classes

### See also

- **hash (C++11)** — hash function object (class template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/hash*
