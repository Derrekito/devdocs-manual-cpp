# swap(std::stop_token)

```cpp
friend void swap( stop_token& lhs, stop_token& rhs ) noexcept;  // (since C++20)
```

Overloads the `std::swap` algorithm for `std::stop_token`. Exchanges the
associated stop-state of `lhs` with that of `rhs`. Effectively calls
`lhs.swap(rhs)`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::stop_token` is an
associated class of the arguments.

### Parameters

- **lhs, rhs** — `stop_token`s to swap

### Return value

(none)

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_token/swap2*
