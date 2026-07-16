# operator==(ranges::join_with_view::*iterator*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x, const /*iterator*/& y )
  requires std::is_reference_v<InnerBase> &&
           std::equality_comparable<ranges::iterator_t<Base>> &&
           std::equality_comparable<ranges::iterator_t<InnerBase>>;  // (since C++23)
```

Compares the underlying iterators. Two iterators are equal if their stored outer
iterators and inner iterators are respectively equal.

The inner iterators may point into either `InnerBase` or `PatternBase`. They
compare equal only if both point into `InnerBase` or both point into
`PatternBase`, and in either case they have the same value.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::join_with_view::iterator<Const>` is an associated class of the
arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x, y** — iterators to compare

### Return value

result of comparison

### See also

- **operator== (C++23)** — compares a sentinel with an iterator returned from
  `join_with_view::begin` (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/iterator/operator_cmp*
