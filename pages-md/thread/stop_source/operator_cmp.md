# operator==(std::stop_source)

```cpp
[[nodiscard]] friend bool operator==( const stop_source& lhs,
                                      const stop_source& rhs ) noexcept;  // (since C++20)
```

Compares two `stop_source` values.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::stop_source` is an
associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **lhs, rhs** — `stop_source`s to compare

### Return value

`true` if `lhs` and `rhs` have the same stop-state, or both have no stop-state,
otherwise `false`.

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_source/operator_cmp*
