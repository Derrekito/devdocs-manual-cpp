# std::hash<std::stacktrace_entry>

```cpp
template<> struct hash<std::stacktrace_entry>;  // (since C++23)
```

The template specialization of `std::hash` for `std::stacktrace_entry` allows
users to obtain hashes of values of type `std::stacktrace_entry`.

`operator()` of this specialization is noexcept.

### Example

### See also

- **hash (C++11)** — hash function object (class template)
- **std::hash<std::basic_stacktrace> (C++23)** — hash support for
  `std::basic_stacktrace` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/utility/stacktrace_entry/hash*
