# operator==(std::stop_token)

```cpp
[[nodiscard]] friend bool operator==( const stop_token& lhs,
                                      const stop_token& rhs ) noexcept;  // (since C++20)
```

Compares two `stop_token` values.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::stop_token` is an
associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **lhs, rhs** — `stop_token`s to compare

### Return value

`true` if `lhs` and `rhs` have the same associated stop-state, or both have no
associated stop-state, otherwise `false`.

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_token/operator_cmp*
