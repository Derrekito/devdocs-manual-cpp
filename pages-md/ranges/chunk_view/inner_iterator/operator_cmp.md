# operator==(ranges::chunk_view::*inner-iterator*)

```cpp
friend constexpr bool operator==( const /*inner-iterator*/& x,
                                  std::default_sentinel_t );  // (since C++23)
```

Compares the iterator and the sentinel.

Let `parent_` be the underlying pointer to enclosing `chunk_view`.

Equivalent to: `returns x.parent_->remainder_ == 0;`

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::chunk_view::inner-iterator` is an associated class of the
arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x** — iterator to compare

### Return value

The result of comparison.

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/inner_iterator/operator_cmp*
