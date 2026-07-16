# std::hash<std::chrono::month_weekday>

```cpp
template<> struct hash<std::chrono::month_weekday>;  // (since C++26)
```

The template specialization of `std::hash` for `std::chrono::month_weekday`
allows users to obtain hashes of objects of type std::chrono::month_weekday.
This specialization is enabled.

The operator() of this specialization is noexcept.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_chrono` | 202306L | (C++26) | Hashing support for `std::chrono`
      value classes

### See also

- **hash (C++11)** — hash function object (class template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/month_weekday/hash*
