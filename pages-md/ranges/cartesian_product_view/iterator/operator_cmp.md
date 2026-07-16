# operator==,<=>(ranges::cartesian_product_view::*iterator*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x, const /*iterator*/& y )
  requires std::equality_comparable<ranges::iterator_t</*maybe-const*/<Const, First>>>;  // (1) (since C++23)
friend constexpr bool operator==( const /*iterator*/& x, std::default_sentinel_t );  // (2) (since C++23)
friend constexpr auto operator<=>( const /*iterator*/& x, const /*iterator*/& y )
  requires /*all-random-access*/<Const, First, Vs...>;  // (2) (since C++23)
```

Compares two iterators or an iterator and a sentinel.

Let `current_` denote the underlying tuple of iterators.

1) Equivalent to: `return x.current_ == y.current_;`

2) Returns `true` if `std::get<i>(x.current_) ==
   ranges::end(std::get<i>(x.parent_->bases_))` is `true` for any integer `0 ≤ i
   ≤ sizeof...(Vs)`. Otherwise, returns `false`.

3) Equivalent to: `return x.current_ <=> y.current_;`

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::cartesian_product_view::iterator<Const>` is an associated class of
the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x, y** — iterators or sentinels to compare

### Return value

The result of comparison.

### Example

### See also

- **operator== (C++23)** — compares a sentinel with an iterator returned from
  `cartesian_product_view::begin` (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/cartesian_product_view/iterator/operator_cmp*
