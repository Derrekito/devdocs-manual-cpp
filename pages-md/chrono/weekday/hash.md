# std::hash<std::chrono::weekday>

```cpp
template<> struct hash<std::chrono::weekday>;  // (since C++26)
```

The template specialization of `std::hash` for `std::chrono::weekday` allows
users to obtain hashes of objects of type std::chrono::weekday. This
specialization is enabled.

The operator() of this specialization is noexcept.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_chrono` | 202306L | (C++26) | Hashing support for `std::chrono`
      value classes

### See also

- **hash (C++11)** — hash function object (class template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/weekday/hash*
