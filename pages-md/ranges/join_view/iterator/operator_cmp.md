# operator==(ranges::join_view::*iterator*, ranges::join_view::*iterator*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x, const /*iterator*/& y )
    requires
        /*ref-is-glvalue*/ &&
        std::equality_comparable<ranges::iterator_t<Base>> &&
        std::equality_comparable<ranges::iterator_t<ranges::range_reference_t<Base>>>;  // (since C++20)
```

Compares the underlying iterators.

Equivalent to: `return (x.outer_ == y.outer_) and (x.inner_ == y.inner_);`,
where `outer_` and `inner_` are the underlying iterators. The constant
`/*ref-is-glvalue*/` in the requires-clause is equal to
`std::is_reference_v<ranges::range_reference_t<Base>>`.

The `!=` operator is synthesized from `operator==`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::join_view::iterator<Const>` is an associated class of the
arguments.

### Parameters

- **x, y** — iterators to compare

### Return value

Result of comparison.

### Example

### See also

- **operator== (C++20)** — compares a sentinel with an iterator returned from
  `join_view::begin` (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_view/iterator/operator_cmp*
