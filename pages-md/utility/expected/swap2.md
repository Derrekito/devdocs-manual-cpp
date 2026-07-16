# swap(std::expected)

```cpp
friend constexpr void swap( expected& lhs, expected& rhs ) noexcept(/*see below*/);  // (since C++23)
```

Overloads the `std::swap` algorithm for `std::expected`. Exchanges the state of
`lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

This overload participates in overload resolution only if `lhs.swap(rhs)` is
valid.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::expected<T, E>` is an
associated class of the arguments.

### Parameters

- **lhs, rhs** — `expected` objects whose states to swap

### Return value

(none)

### Exceptions

`noexcept` specification:

`noexcept(noexcept(lhs.swap(rhs)))`

### Example

### See also

- **swap** — exchanges the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/expected/swap2*
