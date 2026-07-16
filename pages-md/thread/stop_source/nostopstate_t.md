# std::nostopstate_t

```cpp
struct nostopstate_t {
    explicit nostopstate_t() = default;
};  // (since C++20)
```

Unit type intended for use as a placeholder in `std::stop_source` non-default
constructor, that makes the constructed `std::stop_source` empty with no
associated stop-state.

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_source/nostopstate_t*
