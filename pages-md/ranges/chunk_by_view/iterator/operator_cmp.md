# operator==(ranges::chunk_by_view::*iterator*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x, const /*iterator*/& y );  // (1) (since C++23)
friend constexpr bool operator==( const /*iterator*/& x, std::default_sentinel_t );  // (2) (since C++23)
```

Compares the underlying iterators.

1) Equivalent to `return x.current_ == y.current_;`.

2) Equivalent to `return x.current_ == x.next_;`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::chunk_by_view::iterator` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x, y** — iterators to compare

### Return value

The result of comparison.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_by_view/iterator/operator_cmp*
