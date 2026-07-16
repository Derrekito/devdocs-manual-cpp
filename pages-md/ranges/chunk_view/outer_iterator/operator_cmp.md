# operator==(ranges::chunk_view::*outer-iterator*)

```cpp
friend constexpr bool
    operator==( const /*outer-iterator*/& x, std::default_sentinel_t );  // (since C++23)
```

Compares the iterator and the sentinel.

Let `parent_` be the underlying pointer to enclosing `chunk_view`. Equivalent
to:

```cpp
return *x.parent_->current_ == ranges::end(x.parent_->base_) and x.parent_->remainder_ != 0;
```

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::chunk_view::outer-iterator` is an associated class of the
arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x** — iterator to compare

### Return value

The result of comparison.

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/outer_iterator/operator_cmp*
