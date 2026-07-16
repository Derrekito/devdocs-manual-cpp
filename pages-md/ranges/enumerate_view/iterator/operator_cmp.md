# operator==,<=>(ranges::enumerate_view::*iterator*)

```cpp
friend constexpr bool
    operator==( const /*iterator*/& x, const /*iterator*/& y ) noexcept;  // (1) (since C++23)
friend constexpr std::strong_ordering
    operator<=>( const /*iterator*/& x, const /*iterator*/& y ) noexcept;  // (2) (since C++23)
```

Compares the underlying iterators. Let `pos_` be the underlying index.

1) Equivalent to `return x.pos_ == y.pos_;`.

2) Equivalent to `return x.pos_ <=> y.pos_;`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::enumerate_view::iterator<Const>` is an associated class of the
arguments.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.

### Parameters

- **x, y** — iterators to compare

### Return value

Result of comparison.

### See also

- **operator== (C++23)** — compares a sentinel with an iterator returned from
  `enumerate_view::begin` (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view/iterator/operator_cmp*
