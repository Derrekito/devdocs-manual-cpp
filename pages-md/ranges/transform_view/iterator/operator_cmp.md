# operator==,<,>,<=,>=,<=>(ranges::transform_view::*iterator*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x, const /*iterator*/& y )
  requires std::equality_comparable<ranges::iterator_t<Base>>;  // (1) (since C++20)
friend constexpr bool operator<( const /*iterator*/& x, const /*iterator*/& y )
  requires ranges::random_access_range<Base>;  // (2) (since C++20)
friend constexpr bool operator>( const /*iterator*/& x, const /*iterator*/& y )
  requires ranges::random_access_range<Base>;  // (3) (since C++20)
friend constexpr bool operator<=( const /*iterator*/& x, const /*iterator*/& y )
  requires ranges::random_access_range<Base>;  // (4) (since C++20)
friend constexpr bool operator>=( const /*iterator*/& x, const /*iterator*/& y )
  requires ranges::random_access_range<Base>;  // (5) (since C++20)
friend constexpr auto operator<=>( const /*iterator*/& x, const /*iterator*/& y )
  requires ranges::random_access_range<Base> &&
           std::three_way_comparable<ranges::iterator_t<Base>>;  // (6) (since C++20)
```

Compares the underlying iterators.

1) Equivalent to `return x.current_ == y.current_;`, where `current_` is the
   underlying iterator.

2) Equivalent to `return x.current_ < y.current_;`, where `current_` is the
   underlying iterator.

3) Equivalent to `return y < x;`

4) Equivalent to `return !(y < x);`

5) Equivalent to `return !(x < y);`

6) Equivalent to `return x.current_ <=> y.current_;`, where `current_` is the
   underlying iterator.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::transform_view::iterator<Const>` is an associated class of the
arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x, y** — iterators to compare

### Return value

result of comparison

### See also

- **operator== (C++20)** — compares a sentinel with an iterator returned from
  `transform_view::begin` (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view/iterator/operator_cmp*
