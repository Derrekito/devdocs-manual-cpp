# operator==,<=>(ranges::zip_view::*iterator*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x, const /*iterator*/& y )
  requires (std::equality_comparable<
    ranges::iterator_t</*maybe-const*/<Const, Views>>> && ...);  // (1) (since C++23)
friend constexpr auto operator<=>( const /*iterator*/& x, const /*iterator*/& y )
  requires ranges::random_access_range<Base>;  // (2) (since C++23)
```

Compares the underlying iterators.

Let `current_` be the underlying *tuple-like* object of iterators to elements of
adapted views.

1) Returns:

- `x.current_ == y.current_` if `/*all-bidirectional*/<Const, Views...>` is
  true.
- Otherwise, `true` if there exists an integer `0 <= i < sizeof...(Views)` such
  that `bool(std::get<i>(x.current_) == std::get<i>(y.current_))` is true.
- Otherwise, `false`.

2) Equivalent to `return x.current_ <=> y.current_;`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::zip_view::iterator<Const>` is an associated class of the
arguments.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.

### Parameters

- **x, y** — iterators to compare

### Return value

The result of comparison

### See also

- **operator== (C++23)** — compares a sentinel with an iterator returned from
  `zip_view::begin` (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/iterator/operator_cmp*
