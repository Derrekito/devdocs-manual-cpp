# swap(std::stop_source)

```cpp
friend void swap( stop_source& lhs, stop_source& rhs ) noexcept;  // (since C++20)
```

Overloads the `std::swap` algorithm for `std::stop_source`. Exchanges the
stop-state of `lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::stop_source` is an
associated class of the arguments.

### Parameters

- **lhs, rhs** — `stop_source`s to swap

### Return value

(none)

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_source/swap2*
